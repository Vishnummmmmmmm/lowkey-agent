# Vibe-Trading — Complete Codebase Reference

> **Purpose:** This is the single authoritative reference for every file and folder in the Vibe-Trading repository.  
> Use it as the starting point before adding any new feature, debugging, or refactoring.  
> Version: April 2026 · PyPI `vibe-trading-ai` v0.1.5

---

## 1. Repository Root Layout

```
Vibe-Trading/
├── pyproject.toml             # Package config, entry-points, all dependencies
├── Dockerfile                 # Single-container build (Python backend + compiled frontend)
├── docker-compose.yml         # Runs the container on port 8899
├── MANIFEST.in                # Includes .env.example, tests, Docker in sdist
├── .github/                   # CI workflows, PR template
├── agent/                     # ALL Python backend code
└── frontend/                  # React + Vite + TypeScript UI
```

### Key `pyproject.toml` facts

| Field | Value |
|---|---|
| Package name | `vibe-trading-ai` |
| Version | `0.1.5` |
| Python | ≥ 3.11 |
| Entry points | `vibe-trading → cli:main`, `vibe-trading-mcp → mcp_server:main` |
| Package root | `agent/` — so all Python imports are `src.*` / `backtest.*` |
| Skill data files | auto-included via `src/skills/**/*.md\|yaml\|json` glob |

---

## 2. `agent/` — Python Backend Root

```
agent/
├── cli.py                  # Interactive TUI + single-run CLI (66 KB)
├── api_server.py           # FastAPI application (38 KB)
├── mcp_server.py           # FastMCP stdio server — 17 MCP tools (25 KB)
├── SKILL.md                # Skill authoring guide (also used by the agent itself)
├── .env.example            # All supported LLM provider environment variables
├── requirements.txt        # Pinned extras (used in Docker only)
├── backtest/               # Backtest sub-package
├── config/
│   └── swarm/              # 29 YAML swarm preset files
├── src/                    # Main application package
└── tests/                  # Pytest test suite
```

---

## 3. `agent/src/` — Main Application Package

```
src/
├── __init__.py
├── preflight.py            # Startup sanity checks (env vars, optional deps)
├── ui_services.py          # Helpers: build_run_analysis(), load_run_context()
├── agent/                  # ReAct agent core loop
├── core/                   # Run lifecycle + on-disk state
├── memory/                 # Persistent cross-session memory
├── providers/              # LLM client abstraction layer
├── session/                # Multi-turn chat session management
├── shadow_account/         # Virtual paper-trading account + HTML/PDF reports
├── skills/                 # 72 skill directories (each has a SKILL.md)
├── swarm/                  # Multi-agent swarm DAG runtime
└── tools/                  # All callable agent tools (23 files)
```

---

## 4. `src/agent/` — ReAct Core Loop

### Files

| File | Role |
|---|---|
| `loop.py` | **AgentLoop** — the main ReAct loop (789 lines) |
| `context.py` | **ContextBuilder** — assembles system prompt + message history |
| `tools.py` | **BaseTool**, **ToolRegistry** — base classes for all tools |
| `skills.py` | **SkillsLoader** — scans `src/skills/` directories for SKILL.md |
| `memory.py` | **WorkspaceMemory** — in-memory run state (run_dir, tool counters) |
| `frontmatter.py` | YAML frontmatter parser (skills + persistent memory) |
| `trace.py` | **TraceWriter** — writes JSON-lines to `runs/<id>/trace.jsonl` |

---

### `AgentLoop` Execution Flow

```
AgentLoop.run(user_message, history, session_id)
│
├── Create run directory  → runs/<YYYYMMDD_HHMMSS_uuid>/
├── ContextBuilder.build_messages()  → OpenAI-format message list
│
└── LOOP  (max 50 iterations):
    │
    ├── [Layer 1]  _microcompact()         — prune old tool results (keep last 3)
    ├── [Layer 2]  _context_collapse()     — fold long text blocks (zero API cost)
    ├── [Layer 3]  _auto_compact()         — LLM structured summary (if tokens > 40K)
    │
    ├── llm.stream_chat(messages, tools)   → LLMResponse
    │
    ├── IF no tool_calls  → emit final answer, BREAK
    │
    └── _process_tool_calls():
        ├── Handle compact tool   (Layer 4)
        ├── Block duplicate non-repeatable calls
        └── _batch_execute():
            ├── Consecutive readonly tools → parallel threads (up to 8)
            └── Write tools               → serial
```

### 5-Layer Context Compression

| Layer | Trigger | Method | API Cost |
|---|---|---|---|
| 1 — microcompact | Every iteration | Replace old tool results with `[cleared]` | Zero |
| 2 — context_collapse | tokens > 28K | Fold middle of long strings (keep head+tail) | Zero |
| 3 — auto_compact | tokens > 40K | LLM structured 8-section summary | 1 call |
| 4 — compact tool | Model invokes `compact` | Same as Layer 3, model-triggered | 1 call |
| 5 — iterative update | Nth compression | Updates previous summary (zero info decay) | 1 call |

> **When adding tools:** Mark `is_readonly = True` for read-only tools — they get parallel batching and Layer 1 pruning. Mark `repeatable = True` if a tool should be allowed to run more than once per session.

---

### Run Directory Structure

Created per execution at `agent/runs/<run_id>/`:

```
runs/<run_id>/
├── state.json              # {status, created_at, reason}
├── req.json                # {prompt, session_id}
├── trace.jsonl             # JSON-lines event log (every iteration)
├── transcript_<ts>.jsonl   # Full message snapshot before each compression
├── code/
│   └── signal_engine.py    # Agent-generated strategy Python code
└── artifacts/
    ├── equity.csv           # Timestamped equity curve
    ├── trades.csv           # Trade log
    ├── metrics.csv          # 15+ performance metrics (Sharpe, Calmar, etc.)
    ├── validation.json      # Monte Carlo / Bootstrap / Walk-Forward results
    └── strategy.pine        # TradingView PineScript export
```

---

## 5. `src/core/` — Run Lifecycle

| File | Role |
|---|---|
| `runner.py` | **AgentRunner** — wires LLM + registry + AgentLoop |
| `state.py` | **RunStateStore** — creates run dirs, writes `state.json`, `req.json` |

---

## 6. `src/providers/` — LLM Abstraction

| File | Role |
|---|---|
| `llm.py` | **LLMFactory** — reads `LANGCHAIN_PROVIDER`, returns correct LangChain client |
| `chat.py` | **ChatLLM** — wraps LangChain, exposes `chat()` and `stream_chat()` |

**Supported providers** (configured via `.env`):
`openrouter`, `openai`, `deepseek`, `groq`, `ollama`, `dashscope/qwen`, `zhipu`, `moonshot/kimi`, `minimax`, `mimo`, `z-ai`, `gemini`

> Adding a new provider = add a block in `llm.py`'s provider map + document in `.env.example`.

---

## 7. `src/memory/` — Persistent Cross-Session Memory

| File | Role |
|---|---|
| `persistent.py` | **PersistentMemory** — file-based `~/.vibe-trading/memory/` store |

### Storage Layout

```
~/.vibe-trading/memory/
├── MEMORY.md               # Index (≤200 lines) — injected into system prompt
├── user_prefs.md           # Individual memory file with YAML frontmatter
├── project_btc.md
└── ...
```

### Key Methods

| Method | Description |
|---|---|
| `add(name, content, memory_type, description)` | Writes `{type}_{slug}.md` + updates index |
| `remove(name)` | Deletes file + rebuilds index |
| `find_relevant(query)` | Keyword scoring: metadata×2.0 + body×1.0 |
| `.snapshot` property | Frozen index text for system prompt (loaded once at startup) |

> **Design:** Snapshot is frozen at session start (for prompt cache stability). Writes take effect on next session. Supports CJK characters.

---

## 8. `src/tools/` — All Agent Tools (23 files)

### Auto-Discovery

```python
# src/tools/__init__.py — how tools are registered:
# 1. Imports every module in src/tools/ via pkgutil
# 2. Collects all BaseTool subclasses via __subclasses__() BFS
# 3. Calls check_available() to skip tools with missing deps
# → build_registry() returns a ToolRegistry ready for AgentLoop
```

**To add a new tool: Create `src/tools/my_tool.py` extending `BaseTool`. That's it — auto-discovered.**

### Tool Inventory

| File | Tool Name | is_readonly | Description |
|---|---|---|---|
| `backtest_tool.py` | `backtest` | No | Run full backtest pipeline |
| `factor_analysis_tool.py` | `factor_analysis` | Yes | IC/IR factor analysis + quantile backtest |
| `options_pricing_tool.py` | `options_pricing` | Yes | Black-Scholes pricing + full Greeks |
| `pattern_tool.py` | `pattern_recognition` | Yes | Candlestick, harmonic, SMC patterns |
| `swarm_tool.py` | `run_swarm` | No | Start a swarm run from inside agent |
| `load_skill_tool.py` | `load_skill` | Yes | Load SKILL.md file into context |
| `skill_writer_tool.py` | `write_skill` | No | Create/update skill files (self-evolution) |
| `remember_tool.py` | `remember` | No | Save/recall PersistentMemory |
| `session_search_tool.py` | `session_search` | Yes | FTS5 search across all past sessions |
| `web_search_tool.py` | `web_search` | Yes | DuckDuckGo web search |
| `web_reader_tool.py` | `read_url` | Yes | Fetch + parse a URL |
| `doc_reader_tool.py` | `read_document` | Yes | Parse PDF, DOCX, PPTX, XLSX, images |
| `read_file_tool.py` | `read_file` | Yes | Read workspace file (path-sandboxed) |
| `write_file_tool.py` | `write_file` | No | Write workspace file (path-sandboxed) |
| `edit_file_tool.py` | `edit_file` | No | Targeted line-range edits |
| `bash_tool.py` | `bash` | No | Execute shell commands |
| `compact_tool.py` | `compact` | — | Trigger Layer 4 context compression |
| `background_tools.py` | `background_*` | — | Long-running async tasks |
| `shadow_account_tool.py` | `shadow_account` | No | Paper trading account operations |
| `trade_journal_tool.py` | `trade_journal` | No | Parse + analyze trade log CSV/XLSX |
| `path_utils.py` | *(utility)* | — | Safe path containment enforcement |

### `BaseTool` Interface

```python
class BaseTool:
    name: str = ""              # Tool name exposed to LLM
    description: str = ""       # Tool description for LLM
    parameters: dict = {}       # JSON Schema for tool arguments
    is_readonly: bool = False   # True → eligible for parallel batching
    repeatable: bool = False    # True → not blocked on repeated calls

    def run(self, **kwargs) -> str: ...         # Main logic, returns JSON string
    @classmethod
    def check_available(cls) -> bool: ...       # Return False if dep missing
    def get_definition(self) -> dict: ...       # Returns OpenAI-format tool schema
```

---

## 9. `src/session/` — Multi-Turn Chat Sessions

| File | Role |
|---|---|
| `service.py` | **SessionService** — full lifecycle: create, send, run, cancel |
| `store.py` | **SessionStore** — persists sessions/messages/attempts to `agent/sessions/` |
| `models.py` | Dataclasses: `Session`, `Message`, `Attempt`, `AttemptStatus` |
| `events.py` | **EventBus** — async SSE event queue, one queue per session |
| `search.py` | **FTS5 search** across all session messages via SQLite + DuckDB |

### Session Flow

```
POST /sessions                          → SessionService.create_session()
POST /sessions/{id}/messages            → SessionService.send_message()
  → appends Message to store
  → creates Attempt
  → asyncio.create_task(_run_attempt())
      → _run_with_agent()
          → AgentLoop.run()  [ThreadPoolExecutor, max 4 workers]
          → event_callback()  → EventBus.emit()
GET  /sessions/{id}/events              → SSE stream of all events
```

**Session storage path:** `agent/sessions/<session_id>/`

---

## 10. `src/swarm/` — Multi-Agent Swarm

| File | Role |
|---|---|
| `models.py` | `SwarmRun`, `SwarmTask`, `SwarmAgentSpec`, `TaskStatus`, `RunStatus` |
| `runtime.py` | **SwarmRuntime** — DAG executor, one AgentLoop per task (20 KB) |
| `worker.py` | **SwarmWorker** — executes a single task, collects output (17 KB) |
| `presets.py` | `load_preset()`, `list_presets()`, `build_run_from_preset()` |
| `store.py` | Persists `SwarmRun` state to disk |
| `task_store.py` | Per-task result storage |
| `mailbox.py` | Agent-to-agent message passing within a swarm run |
| `api_models.py` | Pydantic models for swarm REST API |

### Swarm Preset YAML Format

```yaml
name: investment_committee
title: Investment Committee
description: Bull/bear debate → risk review → PM final call
variables: [topic]           # User-provided template variables

agents:
  - id: bull_analyst
    role: Bullish analyst
    system_prompt: "You are a bullish analyst focused on upside..."
    tools: [web_search, load_skill, backtest]
    skills: [factor-research, technical-basic]
    max_iterations: 25
    timeout_seconds: 300

tasks:
  - id: bull_case
    agent_id: bull_analyst
    prompt_template: "Build a bull case for: {topic}"
    depends_on: []           # No dependencies → runs immediately
  - id: bear_case
    agent_id: bear_analyst
    prompt_template: "Build a bear case for: {topic}"
    depends_on: []
  - id: final_verdict
    agent_id: portfolio_manager
    prompt_template: "Review: {bull_case.output} vs {bear_case.output}"
    depends_on: [bull_case, bear_case]  # Blocked until both complete
    input_from: {bull_case: output, bear_case: output}
```

### All 29 Swarm Presets (`agent/config/swarm/*.yaml`)

```
investment_committee        global_equities_desk        crypto_trading_desk
earnings_research_desk      macro_rates_fx_desk         quant_strategy_desk
technical_analysis_panel    risk_committee              global_allocation_committee
factor_research_committee   crypto_research_lab         ml_quant_lab
pairs_research_lab          derivatives_strategy_desk   etf_allocation_desk
equity_research_team        fundamental_research_team   fund_selection_panel
credit_research_team        sector_rotation_team        portfolio_review_board
event_driven_task_force     macro_strategy_forum        sentiment_intelligence_team
social_alpha_team           statistical_arbitrage_desk  geopolitical_war_room
convertible_bond_team       commodity_research_team
```

> **Adding a new preset:** Create `agent/config/swarm/<name>.yaml`. Zero code changes — `list_presets()` auto-discovers it.

---

## 11. `src/skills/` — 72 Skill Directories

Each skill = a directory containing `SKILL.md` with YAML frontmatter + markdown instructions.

### Skill Categories

| Category | Count | Key Examples |
|---|---|---|
| Data Source | 6 | `data-routing`, `tushare`, `yfinance`, `okx-market`, `akshare`, `ccxt` |
| Strategy | 17 | `strategy-generate`, `cross-market-strategy`, `technical-basic`, `candlestick`, `ichimoku`, `elliott-wave`, `smc`, `multi-factor`, `ml-strategy` |
| Analysis | 15 | `factor-research`, `macro-analysis`, `global-macro`, `valuation-model`, `earnings-forecast`, `credit-analysis` |
| Asset Class | 9 | `options-strategy`, `options-advanced`, `convertible-bond`, `etf-analysis`, `asset-allocation`, `sector-rotation` |
| Crypto | 7 | `perp-funding-basis`, `liquidation-heatmap`, `stablecoin-flow`, `defi-yield`, `onchain-analysis` |
| Flow | 7 | `hk-connect-flow`, `us-etf-flow`, `edgar-sec-filings`, `financial-statement`, `adr-hshare` |
| Tool | 8 | `backtest-diagnose`, `report-generate`, `pine-script`, `doc-reader`, `web-reader` |

### Adding a New Skill

1. Create `src/skills/<skill-name>/SKILL.md`
2. Add YAML frontmatter:
   ```yaml
   ---
   name: my-skill
   description: One sentence description
   tools: [web_search, read_file]
   ---
   ```
3. Write markdown instructions below the frontmatter
4. Done — auto-discovered by `SkillsLoader`, invokable via `load_skill` tool

---

## 12. `src/shadow_account/` — Paper Trading Account

| File | Role |
|---|---|
| `backtester.py` | Simulated trade execution engine (19 KB) |
| `codegen.py` | Generates trade summary code |
| `extractor.py` | Parses trades from agent text output (13 KB) |
| `reporter.py` | Generates HTML/PDF performance reports via Jinja2 + WeasyPrint |
| `models.py` | `Trade`, `Position`, `AccountState` dataclasses |
| `scanner.py` | Scans run directories for trade data |
| `storage.py` | Persists shadow account state to disk |
| `fonts.py` | Font embedding for PDF reports |
| `templates/` | Jinja2 HTML templates for account reports |

---

## 13. `backtest/` — Backtest Sub-Package

```
backtest/
├── runner.py               # BacktestRunner — top-level orchestrator (18 KB)
├── metrics.py              # 15+ metrics: Sharpe, Calmar, Sortino, VaR, etc. (8 KB)
├── models.py               # BacktestResult, EquityPoint, TradeRecord
├── validation.py           # Monte Carlo, Bootstrap CI, Walk-Forward (12 KB)
├── engines/                # Market-specific execution engines
│   ├── base.py             # BaseEngine — shared OHLCV logic (22 KB)
│   ├── china_a.py          # A-share (T+1, 10% price limits)
│   ├── china_futures.py    # China futures (10 KB)
│   ├── crypto.py           # Crypto (24/7, no shorting restrictions)
│   ├── forex.py            # Forex (leverage, pip calculations)
│   ├── global_equity.py    # US / HK equities
│   ├── global_futures.py   # Global futures (8 KB)
│   ├── options_portfolio.py # Full options portfolio engine (24 KB)
│   ├── composite.py        # Cross-market engine with shared capital pool
│   └── futures_base.py     # Shared futures utilities
├── loaders/                # Data source adapters
│   ├── base.py             # BaseLoader interface
│   ├── registry.py         # Auto-selects best loader per market/symbol
│   ├── akshare_loader.py   # AKShare — A-shares, free (6 KB)
│   ├── yfinance_loader.py  # yfinance — US/HK, free (8 KB)
│   ├── ccxt_loader.py      # CCXT — 100+ crypto exchanges (4 KB)
│   ├── okx.py              # OKX specific (4 KB)
│   ├── tushare.py          # Tushare Pro — token required (6 KB)
│   └── futu.py             # Futu OpenD — HK/A-share, token required (5 KB)
└── optimizers/             # Portfolio optimizers
    ├── base.py             # BaseOptimizer
    ├── mean_variance.py    # Markowitz MVO
    ├── risk_parity.py      # Risk Parity
    ├── equal_volatility.py # Equal Volatility
    └── max_diversification.py  # Max Diversification
```

### Backtest Execution Flow

```
BacktestRunner.run(strategy_code, market, symbols, start, end)
  → loaders/registry.py   → picks best available loader for the market
  → engines/<market>.py   → simulates trades on OHLCV data
  → metrics.py            → computes all 15+ performance metrics
  → validation.py         → Monte Carlo / Bootstrap CI / Walk-Forward
  → writes artifacts/:    equity.csv, trades.csv, metrics.csv, validation.json
```

---

## 14. `api_server.py` — FastAPI Application

**Start:** `vibe-trading serve --port 8899`  
**Interactive docs:** `http://localhost:8899/docs`

### Runs Endpoints (Historical Results)

| Method | Path | Description |
|---|---|---|
| GET | `/runs` | List recent runs (default 20, max 100) |
| GET | `/runs/{run_id}` | Full run details + equity/trades/validation |
| GET | `/runs/{run_id}/code` | Strategy source code (`signal_engine.py`) |
| GET | `/runs/{run_id}/pine` | TradingView PineScript export |

### Session Endpoints (Chat)

| Method | Path | Description |
|---|---|---|
| POST | `/sessions` | Create chat session |
| GET | `/sessions` | List sessions |
| GET | `/sessions/{id}` | Get session |
| DELETE | `/sessions/{id}` | Delete session |
| PATCH | `/sessions/{id}` | Update title |
| POST | `/sessions/{id}/messages` | Send user message → triggers AgentLoop |
| POST | `/sessions/{id}/cancel` | Cancel in-flight agent loop |
| GET | `/sessions/{id}/messages` | Message history |
| GET | `/sessions/{id}/events` | **SSE stream** of live agent events |

### Swarm Endpoints

| Method | Path | Description |
|---|---|---|
| GET | `/swarm/presets` | List all 29 presets |
| POST | `/swarm/runs` | Start a swarm run |
| GET | `/swarm/runs/{id}/events` | SSE stream of swarm progress |
| GET | `/swarm/runs/{id}` | Swarm run status |

### Misc Endpoints

| Method | Path | Description |
|---|---|---|
| GET | `/health` | Liveness probe |
| GET | `/skills` | List all 72 skills |
| GET | `/api` | Service metadata |
| POST | `/upload` | Upload PDF/CSV (max 50 MB) |
| POST | `/system/shutdown` | Local-only (127.0.0.1) graceful shutdown |

### Auth

- Optional: set `API_AUTH_KEY` env var → Bearer token required for all `POST/PUT/PATCH/DELETE`
- If unset: open (dev mode)

### CORS

- Default origins: `localhost:3000`, `localhost:5173`, `localhost:8000`
- Override via `CORS_ORIGINS=http://...,http://...` env var

### SSE Event Types

Events pushed to `/sessions/{id}/events`:
```
session.created   message.received   attempt.created   attempt.started
attempt.completed attempt.failed     attempt.resumed
tool_call         tool_result        text_delta         thinking_done   compact
```

---

## 15. `mcp_server.py` — MCP Plugin (17 Tools)

**Start:** `vibe-trading-mcp` (stdio transport)  
Used with Claude Desktop, Cursor, Windsurf, OpenClaw.

| MCP Tool | Free? | Description |
|---|---|---|
| `list_skills` | ✅ | List all available skills |
| `load_skill` | ✅ | Load skill SKILL.md content |
| `backtest` | ✅ | Run backtest (via yfinance/AKShare/OKX) |
| `factor_analysis` | ✅ | IC/IR factor analysis |
| `analyze_options` | ✅ | Black-Scholes + Greeks |
| `pattern_recognition` | ✅ | Technical pattern detection |
| `get_market_data` | ✅ | Fetch OHLCV data |
| `web_search` | ✅ | DuckDuckGo search |
| `read_url` | ✅ | Fetch URL |
| `read_document` | ✅ | Parse PDF/DOCX/XLSX |
| `read_file` | ✅ | Read file |
| `write_file` | ✅ | Write file |
| `list_swarm_presets` | ✅ | List presets |
| `run_swarm` | ❌ LLM key | Start a swarm run |
| `get_swarm_status` | ✅ | Check swarm status |
| `get_run_result` | ✅ | Fetch run result |
| `list_runs` | ✅ | List recent runs |

---

## 16. `cli.py` — Interactive TUI (66 KB)

The largest single file. Key features:
- **Interactive TUI** (`vibe-trading`) — Rich-based terminal interface with live streaming
- **Single run** (`vibe-trading run -p "..."`) — non-interactive batch mode
- **Slash commands inside TUI:** `/swarm`, `/skills`, `/memory`, `/search`, `/compact`, `/upload`, `/pine`, `/help`
- **Swarm CLI:** `vibe-trading --swarm-run <preset> '{"topic": "TSLA"}'`
- **Export:** `vibe-trading --pine <run_id>` → PineScript / TDX / MT5 formats
- **List presets:** `vibe-trading --swarm-presets`

---

## 17. Frontend (`frontend/`)

**Stack:** React 18 · TypeScript · Vite · TailwindCSS · React Router v6 · shadcn/ui components

```
frontend/src/
├── main.tsx                   # React entry point
├── router.tsx                 # Route definitions (all pages lazy-loaded)
├── index.css                  # Global Tailwind base styles
├── pages/
│   ├── Home.tsx               # Landing / redirect to /agent
│   ├── Agent.tsx              # Main chat + session manager (34 KB — large)
│   ├── RunDetail.tsx          # Detailed run view with charts (10 KB)
│   └── Compare.tsx            # Side-by-side run comparison (13 KB)
├── components/
│   ├── chat/
│   │   ├── AgentAvatar.tsx       # Agent avatar icon
│   │   ├── ConversationTimeline.tsx  # Message thread
│   │   ├── MessageBubble.tsx     # Renders agent messages + inline tool events
│   │   ├── MetricsCard.tsx       # Inline backtest metric panel
│   │   ├── PineScriptViewer.tsx  # Syntax-highlighted PineScript display
│   │   ├── RunCompleteCard.tsx   # Summary card shown after a run finishes
│   │   ├── SwarmDashboard.tsx    # Real-time swarm agent status grid (8 KB)
│   │   ├── ThinkingTimeline.tsx  # Collapsible thinking/reasoning display (6 KB)
│   │   └── WelcomeScreen.tsx     # Empty-state with example prompt buttons (8 KB)
│   ├── charts/
│   │   ├── CandlestickChart.tsx  # Interactive OHLCV candle chart (15 KB)
│   │   ├── EquityChart.tsx       # Equity curve + drawdown (4 KB)
│   │   ├── MiniEquityChart.tsx   # Compact sparkline for list views
│   │   └── ValidationPanel.tsx   # Monte Carlo + Walk-Forward results display
│   ├── layout/
│   │   ├── Layout.tsx            # App shell with sidebar navigation (11 KB)
│   │   └── ConnectionBanner.tsx  # Backend connectivity warning banner
│   └── common/
│       ├── ErrorBoundary.tsx     # React error boundary
│       └── Skeleton.tsx          # Loading skeleton
├── hooks/
│   ├── useSSE.ts              # SSE subscription hook (auto-reconnect, event parsing)
│   └── useDarkMode.ts         # Dark mode toggle + localStorage persistence
├── stores/
│   └── agent.ts               # Zustand store: sessions, messages, runs state
├── lib/                       # Utility helpers (API client, formatters)
└── types/                     # TypeScript interfaces for API responses
```

### Routes

| Path | Component | Description |
|---|---|---|
| `/` | `Home` | Redirects to `/agent` |
| `/agent` | `Agent` | Main chat interface — primary entry point |
| `/runs/:runId` | `RunDetail` | Full run details: charts, trades, metrics |
| `/compare` | `Compare` | Compare two or more runs side-by-side |

### Frontend ↔ Backend Communication

1. **REST API** — standard `fetch()` calls to `http://localhost:8899`
2. **SSE** — `useSSE.ts` subscribes to `/sessions/{id}/events` for real-time agent events
3. **Charts** — data from `GET /runs/{id}` response fields: `equity_curve`, `trade_markers`, `price_series`, `indicator_series`

---

## 18. Environment Variables (`.env.example`)

```bash
# === LLM Provider — pick one block ===
LANGCHAIN_PROVIDER=openrouter
OPENROUTER_API_KEY=sk-or-v1-...
OPENROUTER_BASE_URL=https://openrouter.ai/api/v1
LANGCHAIN_MODEL_NAME=deepseek/deepseek-v3.2

# === Optional data sources ===
TUSHARE_TOKEN=                  # A-share premium data (AKShare is free fallback)
FUTU_HOST=127.0.0.1             # Futu OpenD local host
FUTU_PORT=11111

# === Agent behavior ===
TOKEN_THRESHOLD=40000           # Context compression trigger (tokens)
TIMEOUT_SECONDS=120             # LLM call timeout
ENABLE_SESSION_RUNTIME=true     # Enables /sessions/* API routes

# === API security ===
API_AUTH_KEY=                   # If set, Bearer token required for write endpoints

# === CORS ===
CORS_ORIGINS=http://localhost:5173,http://localhost:3000
```

---

## 19. End-to-End Data Flow

```
User types in frontend chat (Agent.tsx)
    ↓
POST /sessions/{id}/messages            (api_server.py)
    ↓
SessionService.send_message()           creates Attempt
    ↓
asyncio.create_task(_run_attempt())
    ↓
_run_with_agent() → AgentLoop.run()     [ThreadPoolExecutor, max 4 workers]
    ↓
ContextBuilder.build_messages()
  ├── System prompt  (Skills index + PersistentMemory.snapshot)
  ├── Session history  (prior messages trimmed to 12K chars)
  └── Current user message
    ↓
ChatLLM.stream_chat(messages, tool_definitions)
    ↓
IF tool_calls:
  ├── Parallel (readonly): web_search, read_file, factor_analysis, ...
  └── Serial   (write):    backtest, write_file, shadow_account, ...
       ↓
       BacktestRunner (if backtest tool called)
         ├── loaders/registry.py   → picks best loader for symbol/market
         ├── engines/<market>.py   → simulates strategy on historical OHLCV
         ├── metrics.py            → computes 15+ performance metrics
         ├── validation.py         → Monte Carlo / Bootstrap / Walk-Forward
         └── Writes runs/<id>/artifacts/*.csv + validation.json
    ↓
AgentLoop emits events → EventBus → SSE endpoint
    ↓
useSSE.ts (frontend) receives events → updates Zustand store → React re-renders
    ↓
Final LLM answer → Message persisted → SessionStore → UI renders assistant bubble
```

---

## 20. Extension Points — How to Add Features

### New Tool

```python
# agent/src/tools/my_tool.py
from src.agent.tools import BaseTool
import json

class MyTool(BaseTool):
    name = "my_tool"                      # Name the LLM calls
    description = "What this tool does"   # Shown to LLM in system prompt
    is_readonly = True                    # True if no side effects
    repeatable = True                     # True if can call more than once

    parameters = {
        "type": "object",
        "properties": {
            "query": {"type": "string", "description": "Input query"}
        },
        "required": ["query"]
    }

    def run(self, query: str, **kwargs) -> str:
        result = {"data": f"processed: {query}"}
        return json.dumps(result)

    @classmethod
    def check_available(cls) -> bool:
        return True  # Return False if optional dep missing
```
→ Auto-discovered. No registration needed.

### New Backtest Engine

```python
# agent/backtest/engines/my_market.py
from backtest.engines.base import BaseEngine

class MyMarketEngine(BaseEngine):
    market_code = "my_market"
    supported_symbols = ["..."]

    def _apply_market_rules(self, signals, prices, **kwargs):
        # Override T+1, leverage, tick rules, etc.
        return signals
```
→ Register in `backtest/engines/__init__.py` engine map.

### New Data Loader

```python
# agent/backtest/loaders/my_source.py
from backtest.loaders.base import BaseLoader
import pandas as pd

class MySourceLoader(BaseLoader):
    source_name = "mysource"

    def load(self, symbol: str, start: str, end: str, interval: str) -> pd.DataFrame:
        # Return OHLCV DataFrame with columns: open, high, low, close, volume
        ...
```
→ Register in `backtest/loaders/registry.py`.

### New Swarm Preset

Create `agent/config/swarm/my_team.yaml` following the YAML schema above.  
→ Zero code changes. Discoverable immediately via `list_presets()`.

### New Skill

Create `agent/src/skills/<skill-name>/SKILL.md` with YAML frontmatter.  
→ Discoverable via `SkillsLoader`. Callable via `load_skill` tool.

### New API Endpoint

Add route directly to `agent/api_server.py`:
```python
@app.get("/my/endpoint")
async def my_endpoint():
    return {"result": "..."}

# For write endpoints requiring auth:
@app.post("/my/endpoint", dependencies=[Depends(require_auth)])
async def my_write_endpoint(body: MyModel):
    ...
```

### New Frontend Page

1. Create `frontend/src/pages/MyPage.tsx`
2. Add to `frontend/src/router.tsx`:
   ```tsx
   const MyPage = lazy(() => import("@/pages/MyPage").then(m => ({ default: m.MyPage })));
   // Add to children array:
   { path: "/mypath", element: wrap(MyPage) }
   ```

---

## 21. Runtime Paths Reference

| Path | Description |
|---|---|
| `agent/runs/` | All agent run outputs (one dir per run) |
| `agent/sessions/` | Chat session JSON persistence |
| `agent/uploads/` | Uploaded PDF/CSV files |
| `~/.vibe-trading/memory/` | Persistent cross-session memory files |
| `agent/src/skills/` | All skill SKILL.md directories |
| `agent/config/swarm/` | All swarm preset YAML files |

---

## 22. Testing

```bash
# Run all tests
pytest

# Run only fast unit tests (no network)
pytest -m unit

# Run integration tests
pytest -m integration

# Run specific file
pytest agent/tests/test_<name>.py -v
```

Config in `pyproject.toml`:
```toml
[tool.pytest.ini_options]
testpaths = ["agent/tests"]
pythonpath = ["agent"]
markers = [
  "unit: Unit tests (fast, no network)",
  "integration: Integration tests (may need network)"
]
```

---

## 23. Docker

```bash
docker compose up --build      # Build + start at http://localhost:8899
docker compose down            # Stop and remove containers
```

`Dockerfile` multi-stage:
1. **Stage 1** — Node.js: builds the React frontend into `frontend/dist/`
2. **Stage 2** — Python: installs backend deps, copies frontend dist into static dir
3. **CMD** — `uvicorn api_server:app --host 0.0.0.0 --port 8899`

FastAPI serves both the REST API and the compiled React SPA from the same port.

---

## 24. Full Dependency Map

| Category | Key Libraries |
|---|---|
| Agent / LLM | `langchain`, `langchain-openai`, `langchain-core`, `langgraph` |
| API Server | `fastapi`, `uvicorn[standard]`, `pydantic`, `sse-starlette`, `python-multipart` |
| MCP | `fastmcp` |
| Data — Equities | `yfinance`, `akshare`, `tushare` |
| Data — Crypto | `ccxt` |
| Data — A-share | `akshare`, `tushare` |
| Data — HK | `futu` (optional), `yfinance` |
| Quantitative | `scipy`, `scikit-learn`, `numpy`, `pandas`, `smartmoneyconcepts`, `pyharmonics` |
| Documents | `pypdfium2`, `python-docx`, `python-pptx`, `openpyxl`, `Pillow` |
| Viz / Reports | `matplotlib`, `weasyprint`, `jinja2` |
| Search | `duckdb` (FTS5 session search), `ddgs` (web search) |
| DB | `duckdb` |
| Env | `python-dotenv`, `pyyaml`, `rich` |
| Frontend | React 18, Vite, TailwindCSS, React Router v6, Zustand, shadcn/ui |

---

*Auto-generated by full file-tree inspection · April 2026*
