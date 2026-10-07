<div align="center">

<img src="https://img.shields.io/badge/-%E2%9A%A1%EF%B8%8F%20SUPREME%20TRADING%20AGI%20(LAMAGENT)-0f172a?style=for-the-badge&labelColor=020617" alt="Supreme Trading AGI Banner" width="520"/>

<br/>
<br/>

# 🏛️ Supreme Trading AGI (LAMAgent)
### The Sovereign Neural Platform for Global Financial & Quantitative Intelligence
**An Autonomous Multi-Agent AGI Ecosystem for Strategy Synthesis, Quantitative Research & Institutional Backtesting**

<br/>

[![Python 3.11+](https://img.shields.io/badge/Python-3.11%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI Backend](https://img.shields.io/badge/Backend-FastAPI%20Async-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React 19 Frontend](https://img.shields.io/badge/Frontend-React%2019%20%7C%20Vite%20%7C%20Tailwind-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![Swarm Presets](https://img.shields.io/badge/Swarm%20Teams-29%20Active-7C3AED?style=for-the-badge&logo=anthropic&logoColor=white)](#-the-29-swarm-neural-registry)
[![Neural Skills](https://img.shields.io/badge/Neural%20Skills-72%20Domains-EA580C?style=for-the-badge)](#-72-domain-skills--27-sandboxed-tools)
[![Backtest Engines](https://img.shields.io/badge/Backtest%20Engines-7%20Markets-059669?style=for-the-badge)](#-institutional-backtesting--statistical-validation)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

<br/>

> **"Democratizing the Institutional Quantitative Hedge Fund."**  
> Zero-latency, mathematically grounded quantitative research, automated strategy synthesis, cross-market backtesting, and autonomous multi-agent execution — accessible to every trader, quantitative researcher, and institutional desk worldwide.

</div>

---

## 📑 Table of Contents
1. [🏛️ Project Vision & Institutional Thesis](#️-project-vision--institutional-thesis)
2. [🗺️ Live System Architecture Overview](#️-live-system-architecture-overview)
3. [🤖 The 29-Swarm Neural Registry](#-the-29-swarm-neural-registry)
4. [🧠 72 Domain Skills & 27 Sandboxed Tools](#-72-domain-skills--27-sandboxed-tools)
5. [🛠️ Technical Stack Matrix](#️-technical-stack-matrix)
6. [🗂️ Repository & Directory Architecture](#️-repository--directory-architecture)
7. [⚡ Core Technical Highlights & Deep Dives](#-core-technical-highlights--deep-dives)
   - [1. Multi-Agent Swarm DAG Runtime & Intent Triage](#1-multi-agent-swarm-dag-runtime--intent-triage)
   - [2. Institutional Backtesting & Statistical Validation Kernel](#2-institutional-backtesting--statistical-validation-kernel)
   - [3. 5-Layer Context Compression & Autonomous Skill Evolution](#3-5-layer-context-compression--autonomous-skill-evolution)
   - [4. Cross-Market Composite Engine & Universal Data Routing](#4-cross-market-composite-engine--universal-data-routing)
   - [5. Multi-Platform Strategy Compiler (TradingView, MT5, TDX, VNPY)](#5-multi-platform-strategy-compiler-tradingview-mt5-tdx-vnpy)
   - [6. Forensic Trade Journal Parsing & Shadow Account Sandbox](#6-forensic-trade-journal-parsing--shadow-account-sandbox)
8. [🚀 Local Execution & Quickstart Guide](#-local-execution--quickstart-guide)
9. [🔌 FastMCP Integration (Claude Desktop & Cursor)](#-fastmcp-integration-claude-desktop--cursor)
10. [🔒 Security, Sandboxing & Production Hardening](#-security-sandboxing--production-hardening)
11. [🏆 Innovation Context & Quantitative Impact Thesis](#-innovation-context--quantitative-impact-thesis)
12. [🔮 Future Development & Research Roadmap](FUTURE_DEVELOPMENT.md)

---

## 🏛️ Project Vision & Institutional Thesis

**Supreme Trading AGI (LAMAgent)** is a production-grade autonomous quantitative intelligence ecosystem — not a superficial chatbot. It functions as a complete digital **Hedge Fund Investment Committee and Quantitative Engineering Desk**, fusing symbolic quantitative algorithms, deterministic backtesting engines, multi-agent swarm orchestration, and multi-modal financial document intelligence.

### Problem vs. Our Solution Matrix

| Critical Industry Failure | Supreme Trading AGI Solution |
|---|---|
| **Elite quant advisory is locked behind ₹100M+ AUM minimums** | **29 specialized multi-agent swarms** replicate tier-1 hedge fund strategy desks at zero marginal cost. |
| **LLMs hallucinate indicators and fake backtest returns** | **Deterministic Python Execution Kernels** run real vectorized calculations (NumPy/Pandas/DuckDB) with zero hallucinated math. |
| **Backtest overfitting and regime fragility** | Built-in **Monte Carlo (10,000 paths), Bootstrap Confidence Intervals, and Walk-Forward** validation matrices. |
| **Fragmented global data feeds across disparate markets** | Unified zero-config data router automatically connects and falls back across **AkShare, yfinance, CCXT, OKX, Tushare Pro, and Futu OpenD**. |
| **Strategy execution trapped inside closed codebases** | **Universal Strategy Compiler** compiles logic to **TradingView Pine Script v6, MetaTrader 5 (MQL5), TDX (通达信), and vn.py CtaTemplate**. |
| **Trader behavioral leaks and unrecorded edge erosion** | **Trade Journal Forensic Analyzer** detects disposition effect, overtrading, chasing momentum, and runs **counterfactual shadow backtests**. |

---

## 🗺️ Live System Architecture Overview

```
┌───────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                     SUPREME TRADING AGI PLATFORM                                      │
│                                                                                                       │
│   ┌────────────────────────┐         ┌────────────────────────┐         ┌────────────────────────┐    │
│   │   React 19 Frontend    │────────▶│   FastAPI Orchestrator │────────▶│ 13 LLM Provider Mesh   │    │
│   │   Vite + TypeScript    │         │   (api_server.py)      │         │ DeepSeek / OpenAI /    │    │
│   │   SSE Real-Time Stream │◀────────│   ReAct AgentLoop      │◀────────│ Claude / Qwen / Kimi   │    │
│   └────────────────────────┘         └───────────┬────────────┘         └────────────────────────┘    │
│                                                   │                                                   │
│                        ┌──────────────────────────┼──────────────────────────┐                        │
│                        ▼                          ▼                          ▼                        │
│             ┌────────────────────┐     ┌────────────────────┐     ┌────────────────────┐              │
│             │  29 Swarm Presets  │     │ 72 Domain Skills   │     │ 27 Sandboxed Tools │              │
│             │  DAG Runtime Engine│     │ SMC, Harmonics,    │     │ Backtest, Factors, │              │
│             │  Mailbox Messaging │     │ Macro, Options     │     │ Search, Doc Parser │              │
│             └────────────────────┘     └────────────────────┘     └────────────────────┘              │
│                        │                          │                          │                        │
│                        └──────────────────────────┼──────────────────────────┘                        │
│                                                   │                                                   │
│                        ┌──────────────────────────┴──────────────────────────┐                        │
│                        ▼                                                     ▼                        │
│             ┌───────────────────────────────────┐         ┌───────────────────────────────────┐       │
│             │ 7 Institutional Backtest Engines  │         │ Universal Multi-Platform Compiler │       │
│             │ • China A-Shares (T+1, limits)    │         │ • TradingView Pine Script v6      │       │
│             │ • Global Equities (US / HK)       │         │ • MetaTrader 5 (MQL5)             │       │
│             │ • Crypto (24/7 Futures & Spot)    │         │ • TDX (通达信 / 同花顺)           │       │
│             │ • Forex & Commodity Futures       │         │ • vn.py CtaTemplate (Python)      │       │
│             │ • Options Portfolio & Greeks      │         │ • Automated PDF/HTML Factsheets   │       │
│             │ • Cross-Market Composite Engine   │         └───────────────────────────────────┘       │
│             └───────────────────────────────────┘                                                     │
│                                                   │                                                   │
│                        ┌──────────────────────────┴──────────────────────────┐                        │
│                        ▼                                                     ▼                        │
│             ┌───────────────────────────────────┐         ┌───────────────────────────────────┐       │
│             │ Universal Data Router & Adapters  │         │ Memory & Forensic Audit Registry  │       │
│             │ AkShare • yfinance • CCXT • OKX   │         │ Persistent YAML Store • DuckDB    │       │
│             │ Tushare Pro • Futu OpenD          │         │ SQLite FTS5 Search • trace.jsonl  │       │
│             └───────────────────────────────────┘         └───────────────────────────────────┘       │
└───────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🤖 The 29-Swarm Neural Registry

The platform features **29 pre-configured autonomous multi-agent swarm teams** defined via declarative DAGs (`agent/config/swarm/*.yaml`). Each swarm coordinates specialized sub-agents with dedicated roles, tools, and message passing:

| Swarm ID | Team Name | Domain Focus | Core Autonomous Mission |
|---|---|---|---|
| `SW-01` | **Investment Committee** | Multi-Asset Asset Allocation | Adversarial Bull/Bear debate, macro overlay, risk review, and Portfolio Manager verdict. |
| `SW-02` | **Quant Strategy Desk** | Algorithmic Alpha | End-to-end signal engineering, quantitative formulation, and backtest code synthesis. |
| `SW-03` | **Technical Analysis Panel** | Confluence Charting | Multi-timeframe confluence: Smart Money Concepts (SMC), Harmonic patterns, and Elliott Wave. |
| `SW-04` | **Risk Committee** | Quantitative Risk | Extreme tail-risk budgeting, parametric/historical VaR, Expected Shortfall (CVaR), and draw-down caps. |
| `SW-05` | **Statistical Arbitrage Desk** | Mean Reversion | Cointegration screening, Johansen tests, rolling z-score spread tracking, and pairs execution. |
| `SW-06` | **Pairs Research Lab** | Relative Value | Dynamic hedge-ratio calculation (Kalman filter vs OLS), half-life estimation, and spread trading. |
| `SW-07` | **ML Quant Lab** | Machine Learning Alpha | Feature extraction, Gradient Boosting / Random Forest alpha signals, and SHAP feature attribution. |
| `SW-08` | **Derivatives Strategy Desk** | Options & Volatility | Options payoff matrix synthesis, implied volatility skew mapping, and Black-Scholes Greeks hedging. |
| `SW-09` | **Crypto Trading Desk** | Digital Assets | 24/7 perpetual futures, funding rate arbitrage, basis trading, and cross-exchange liquidity. |
| `SW-10` | **Crypto Research Lab** | Web3 Telemetry | On-chain capital flows, token unlock inflation schedules, smart contract metrics, and DeFi yields. |
| `SW-11` | **Macro Rates & FX Desk** | Sovereign Rates & FX | Central bank policy trajectories (Fed, ECB, PBOC, RBI), yield curve steepeners, and FX cross hedging. |
| `SW-12` | **Macro Strategy Forum** | Global Macro | Economic regime transition detection, stagflation/expansion cycle mapping, and global asset rotation. |
| `SW-13` | **Geopolitical War Room** | Sovereign & Defense Risk | Conflict heatmaps, trade corridor disruption modeling, sanction impacts, and supply chain shocks. |
| `SW-14` | **Equity Research Team** | Fundamental Equities | Bottom-up company screening, competitive moat auditing, forensic accounting, and DCF valuation. |
| `SW-15` | **Fundamental Research Team** | Financial Modeling | Financial statement normalization, Dupont analysis, return on invested capital (ROIC), and cash burn. |
| `SW-16` | **Earnings Research Desk** | Pre/Post Earnings Alpha | Consensus revision momentum, earnings surprise distribution, and Post-Earnings Announcement Drift (PEAD). |
| `SW-17` | **Event Driven Task Force** | Corporate Catalysts | Merger arbitrage, tender offers, index rebalancing inclusions/exclusions, and spin-off mispricings. |
| `SW-18` | **Factor Research Committee** | Quantitative Factors | Fama-French 5-factor analysis, Information Coefficient (IC/IR) decay, and factor turnover decay. |
| `SW-19` | **Sector Rotation Team** | Industry Rotation | Intermarket business cycle phase identification and ETF relative strength momentum allocation. |
| `SW-20` | **ETF Allocation Desk** | Thematic & Beta | Core-satellite ETF portfolio construction, liquidity screening, and tracking error minimization. |
| `SW-21` | **Credit Research Team** | Fixed Income | High-yield vs IG credit spreads, debt maturity wall assessment, and recovery rate modeling. |
| `SW-22` | **Convertible Bond Team** | Hybrid Securities | Conversion premium modeling, delta sensitivity, bond floor valuation, and gamma arbitrage. |
| `SW-23` | **Commodity Research Team** | Hard & Soft Commodities | Supply/demand balance sheets, inventory storage curves, backwardation/contango term structures. |
| `SW-24` | **Fund Selection Panel** | Manager Selection | Mutual fund / hedge fund alpha attribution, manager style consistency, and drawdown recovery duration. |
| `SW-25` | **Global Equities Desk** | Cross-Border Equities | US/HK/A-share/ADR parity trading, foreign exchange pass-through, and global market beta alignment. |
| `SW-26` | **Global Allocation Committee** | Endowment / SWF Model | Multi-decade strategic asset allocation, risk-parity weightings, and real-return preservation. |
| `SW-27` | **Portfolio Review Board** | Mandate Governance | Factor tilt monitoring, tracking error constraint verification, and mandate rebalancing decisions. |
| `SW-28` | **Sentiment Intelligence Team** | Market Sentiment | Multimodal news NLP sentiment, options put/call ratio sentiment, and market fear-greed divergences. |
| `SW-29` | **Social Alpha Team** | Crowd & Retail Dynamics | High-velocity retail social sentiment scraping, social momentum surges, and crowd positioning squeeze alerts. |

---

## 🧠 72 Domain Skills & 27 Sandboxed Tools

### 72 Neural Financial Skills (`agent/src/skills/`)
Each skill is a self-contained, auto-discovered domain package containing structured rules, calculation models, and execution recipes:

- **Quantitative Strategies (17):** `strategy-generate`, `cross-market-strategy`, `smc` (Smart Money Concepts), `harmonic` (Gartley, Bat, Butterfly), `candlestick`, `ichimoku`, `elliott-wave`, `chanlun`, `multi-factor`, `ml-strategy`, `pair-trading`, `volatility`, `seasonal`, `minute-analysis`, `execution-model`, `hedging-strategy`, `technical-basic`.
- **Valuation & Fundamentals (15):** `valuation-model`, `financial-statement`, `fundamental-filter`, `earnings-forecast`, `earnings-revision`, `credit-analysis`, `factor-research`, `macro-analysis`, `global-macro`, `correlation-analysis`, `performance-attribution`, `quant-statistics`, `risk-analysis`, `behavioral-finance`, `geopolitical-risk`.
- **Asset Classes & Derivatives (9):** `options-strategy`, `options-advanced`, `options-payoff`, `convertible-bond`, `etf-analysis`, `asset-allocation`, `sector-rotation`, `commodity-analysis`, `fund-analysis`.
- **Crypto & Web3 Intelligence (7):** `crypto-derivatives`, `perp-funding-basis`, `liquidation-heatmap`, `stablecoin-flow`, `token-unlock-treasury`, `defi-yield`, `onchain-analysis`.
- **Institutional Flows & Fillings (7):** `hk-connect-flow` (Northbound/Southbound), `us-etf-flow`, `edgar-sec-filings` (10-K, 10-Q, 8-K), `adr-hshare`, `corporate-events`, `market-microstructure`, `regulatory-knowledge`.
- **Data Connectors & Routers (6):** `data-routing`, `tushare`, `yfinance`, `okx-market`, `akshare`, `ccxt`.
- **Tooling & Platform Compilers (11):** `backtest-diagnose`, `pine-script`, `vnpy-export`, `trade-journal`, `shadow-account`, `doc-reader`, `web-reader`, `report-generate`, `social-media-intelligence`, `sentiment-analysis`.

### 27 Sandboxed Execution Tools (`agent/src/tools/`)
Tools feature **parallel batching for read-only calls (up to 8 threads)** and **strict path containment sandboxing**:

```
[Execution & Backtesting]   backtest • factor_analysis • options_pricing • pattern_recognition • run_swarm
[Memory & Evolution]        remember • session_search (FTS5) • load_skill • write_skill (self-evolution)
[Journal & Shadow Sandbox]  trade_journal • shadow_account • trade_journal_parsers
[Document & Web Ingestion]  read_document (PDF/Word/PPT/Excel/OCR) • web_search • read_url
[System & Sandboxed Files]  read_file • write_file • edit_file • bash • compact • background_*
```

---

## 🛠️ Technical Stack Matrix

```
┌────────────────────────────────────────────────────────────────────────────────────────────┐
│ FRONTEND LAYER                                                                             │
│ • React 19 (Modern Concurrent Features)        • Tailwind CSS (Glassmorphism Dark UI)      │
│ • TypeScript (Strict Type Safety)             • Vite (Ultra-fast HMR & Optimized Bundles)  │
│ • Lucide Icons & Radix UI Primitives          • Server-Sent Events (SSE) Real-Time Stream  │
├────────────────────────────────────────────────────────────────────────────────────────────┤
│ BACKEND & ORCHESTRATION LAYER                                                              │
│ • FastAPI & Uvicorn (Asynchronous REST & SSE) • Python 3.11+ (High Performance Runtime)    │
│ • LangGraph (Multi-Agent Swarm DAGs)          • ReAct AgentLoop (50-step bounded loop)     │
│ • FastMCP 2.0 (Claude Desktop / Cursor StdIO) • ThreadPoolExecutor (Parallel batching)     │
├────────────────────────────────────────────────────────────────────────────────────────────┤
│ QUANTITATIVE & BACKTESTING ENGINES                                                         │
│ • NumPy & Pandas 2.0 (Vectorized Operations)  • SciPy (Statistical & Matrix Solvers)       │
│ • DuckDB 1.2+ (Ultra-fast OLAP Query Engine)  • SmartMoneyConcepts & PyHarmonics           │
│ • Black-Scholes & Greeks Analytical Engine    • 4 Optimizers (Markowitz, Risk Parity, etc.)│
├────────────────────────────────────────────────────────────────────────────────────────────┤
│ GLOBAL MARKET DATA CONNECTORS                                                              │
│ • yfinance (US & Global Equities, Indices)    • AkShare (China A-Shares, Macro Data)       │
│ • CCXT (100+ Crypto Exchanges: Binance, etc.) • OKX v5 API (Perpetuals, Spot, Orderbook)   │
│ • Tushare Pro (Institutional China Equities)  • Futu OpenD (HK & US Real-time Level 2)     │
├────────────────────────────────────────────────────────────────────────────────────────────┤
│ EXPORT & COMPILER TARGETS                                                                  │
│ • TradingView Pine Script v6                  • MetaTrader 5 (MQL5 Automated EAs)          │
│ • TongDaXin (通达信) / EastMoney Formulas     • vn.py CtaTemplate (Python Quant Trading)   │
│ • Jinja2 + WeasyPrint (Automated PDF Reports) • Interactive HTML Factsheets                │
└────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🗂️ Repository & Directory Architecture

```
lamagent/
├── agent/                                 # Complete Python Backend Engine
│   ├── api_server.py                      # FastAPI REST & SSE Streaming Server (38 KB)
│   ├── cli.py                             # Interactive Terminal UI & Command Runner (66 KB)
│   ├── mcp_server.py                      # FastMCP stdio server for Claude / Cursor (25 KB)
│   ├── SKILL.md                           # Autonomous skill authoring standard
│   ├── .env.example                       # All 13 supported LLM provider configurations
│   ├── requirements.txt                   # Production Python package manifest
│   │
│   ├── backtest/                          # Quantitative Backtesting Sub-Package
│   │   ├── runner.py                      # BacktestRunner top-level orchestrator
│   │   ├── metrics.py                     # 15+ performance metrics (Sharpe, Calmar, VaR)
│   │   ├── validation.py                  # Monte Carlo, Bootstrap CI, Walk-Forward
│   │   ├── models.py                      # BacktestResult, TradeRecord, EquityPoint
│   │   ├── engines/                       # 7 Market-Specific Execution Engines
│   │   │   ├── base.py                    # Shared vectorized OHLCV execution core
│   │   │   ├── china_a.py                 # A-Share rules (T+1 settlement, 10% limit)
│   │   │   ├── global_equity.py           # US & Hong Kong equity mechanics
│   │   │   ├── crypto.py                  # 24/7 continuous crypto trading & leverage
│   │   │   ├── china_futures.py           # Margin requirements & rollover adjustments
│   │   │   ├── global_futures.py          # CME/NYMEX ticks, tick values & contracts
│   │   │   ├── forex.py                   # Currency lots, pip math & floating leverage
│   │   │   ├── options_portfolio.py       # Portfolio Greeks, strike rolls, payoffs
│   │   │   └── composite.py               # Cross-market portfolio with shared capital
│   │   ├── loaders/                       # Data Feed Adapters
│   │   │   ├── registry.py                # Automatic provider selector & fallback
│   │   │   ├── akshare_loader.py          # Free China A-shares & ETF historical data
│   │   │   ├── yfinance_loader.py         # US/Global equities, commodities, FX
│   │   │   ├── ccxt_loader.py             # 100+ crypto exchanges normalized
│   │   │   ├── okx.py                     # OKX direct market data loader
│   │   │   ├── tushare.py                 # Tushare Pro token-authenticated feed
│   │   │   └── futu.py                    # Futu OpenD high-speed gateway
│   │   └── optimizers/                    # 4 Portfolio Optimizers
│   │       ├── mean_variance.py           # Markowitz Mean-Variance Optimization
│   │       ├── risk_parity.py             # Equal Risk Contribution (ERC)
│   │       ├── equal_volatility.py        # Volatility-weighted capital allocation
│   │       └── max_diversification.py     # Maximum Diversification Ratio (MDR)
│   │
│   ├── config/
│   │   └── swarm/                         # 29 Declarative Swarm Presets (.yaml)
│   │
│   └── src/                               # Application Core Package
│       ├── agent/                         # ReAct Engine: Loop, ContextBuilder, Trace
│       ├── memory/                        # Persistent cross-session memory (~/.vibe-trading/)
│       ├── providers/                     # 13 LLM Provider drivers (OpenAI, DeepSeek, etc.)
│       ├── session/                       # Multi-turn chat sessions with SQLite FTS5 search
│       ├── shadow_account/                # Paper account simulator & PDF/HTML reports
│       ├── skills/                        # 72 Deep financial skills (each has SKILL.md)
│       ├── swarm/                         # Multi-Agent Swarm DAG Runtime & Mailbox
│       └── tools/                         # 27 Sandboxed agent execution tools
│
├── frontend/                              # Sentient Dark UI Web Application
│   ├── src/                               # React 19 + TypeScript source components
│   ├── public/                            # Static media and brand assets
│   ├── package.json                       # Node dependencies and scripts
│   ├── vite.config.ts                     # Optimized Vite build configuration
│   └── tailwind.config.ts                 # Cinematic styling system tokens
│
├── assets/                                # Badges, diagrams, and UI imagery
├── CODEBASE_REFERENCE.md                  # Comprehensive architectural specification
├── Dockerfile                             # Production container (Backend + compiled UI)
├── docker-compose.yml                     # Single-command orchestration
├── pyproject.toml                         # Packaging specification & entry-points
└── README.md                              # Institutional Platform Documentation
```

---

## ⚡ Core Technical Highlights & Deep Dives

### 1. Multi-Agent Swarm DAG Runtime & Intent Triage
Standard chat systems break down when analyzing complex financial queries. Supreme Trading AGI utilizes a **Directed Acyclic Graph (DAG) Swarm Engine** (`agent/src/swarm/runtime.py`):
- **Declarative Task Graphs:** Dependencies are modeled explicitly in YAML (`depends_on: [bull_case, bear_case]`). Independent tasks run in parallel across worker threads.
- **Agent Mailbox Architecture:** Specialized sub-agents communicate via structured message passing (`mailbox.py`), allowing a portfolio manager to ingest inputs from bull, bear, and risk analysts simultaneously.
- **Autonomous Intent Triage:** Incoming prompts are routed dynamically to the optimal swarm team or individual specialist agent based on market regime and instrument scope.

### 2. Institutional Backtesting & Statistical Validation Kernel
Standard LLM trading projects emit code without verifying statistical robustness. Supreme Trading AGI runs a deterministic Python testing suite:
- **Vectorized Backtest Engine:** Zero LLM math. Real trades are simulated against high-resolution OHLCV series with slippage, commissions, and market rules.
- **Monte Carlo Simulation (10,000 Runs):** Shuffles trade sequences and tests returns against random walk equity curves to calculate the Probability of Maximum Drawdown ($P_{\text{MDD}}$) and Ruin Probability.
- **Bootstrap Confidence Intervals:** Generates 95% confidence intervals for Sharpe Ratio, Calmar Ratio, Sortino Ratio, and Win Rate.
- **Walk-Forward Validation:** Splits historical series into rolling train/test windows to detect overfitting and parameter decay.

```
Input: Cross-Market Momentum Strategy (BTC + NVDA + Gold)
  │
  ├── Loader Registry → Pulls CCXT (BTC) + yfinance (NVDA, GLD)
  ├── Composite Engine → Simulates unified portfolio with shared capital pool
  ├── Execution Simulation → Applies market-specific fees, slippage & settlement
  ├── Metrics Engine → Computes Sharpe, Sortino, Calmar, VaR (95%/99%)
  ├── Validation Suite → 10,000 Monte Carlo iterations + Walk-Forward test
  └── Artifact Generation → equity.csv, trades.csv, validation.json, strategy.pine
```

### 3. 5-Layer Context Compression & Autonomous Skill Evolution
To prevent LLM context saturation during 50-turn quantitative research sessions without token bloat, the platform implements a **5-Layer Context Compression Pipeline**:

| Compression Layer | Trigger Threshold | Technique | API Cost |
|---|---|---|---|
| **Layer 1: Microcompact** | Every turn | Prunes historical tool outputs, keeping only the 3 most recent | **Zero** |
| **Layer 2: Context Collapse** | Tokens > 28,000 | Folds the center of long JSON/tables while retaining head & tail | **Zero** |
| **Layer 3: Auto-Compact** | Tokens > 40,000 | Synthesizes an 8-section structured financial research summary | 1 LLM Call |
| **Layer 4: Model Compact** | Agent-triggered | Model invokes the `compact` tool upon reaching milestone | 1 LLM Call |
| **Layer 5: Iterative Update** | Subsequent runs | Progressively updates prior summaries without memory decay | 1 LLM Call |

**Autonomous Skill Evolution:** When the agent discovers a novel pattern or user workflow, it utilizes `write_skill` to author a new directory in `agent/src/skills/` complete with YAML frontmatter, self-evolving its capabilities for future sessions.

### 4. Cross-Market Composite Engine & Universal Data Routing
- **Composite Capital Engine (`backtest/engines/composite.py`):** Backtests mixed-asset portfolios (e.g., A-Shares + US Equities + Crypto + Forex) with a single shared capital pool, rebalancing dynamically while enforcing individual market constraints (T+1 settlement vs 24/7 continuous trading).
- **Auto-Fallback Data Router:** If a primary data source experiences downtime or rate limits, the loader registry dynamically fails over (e.g., `tushare` $\rightarrow$ `akshare` $\rightarrow$ `yfinance`).

### 5. Multi-Platform Strategy Compiler (TradingView, MT5, TDX, VNPY)
Strategies authored in natural language or Python are compiled into target execution languages via dedicated syntax synthesis engines:
- **TradingView Pine Script v6:** Full indicators, strategy alert conditions, plot overlays, and backtest parameter inputs.
- **MetaTrader 5 (MQL5):** Object-oriented Expert Advisors (`.mq5`) with stop-loss, take-profit, and lot sizing logic.
- **TDX / EastMoney (通达信/同花顺):** Native formula scripts for domestic Chinese charting terminals.
- **vn.py CtaTemplate:** Production-grade Python event-driven CTA strategies ready for live broker connectivity.

### 6. Forensic Trade Journal Parsing & Shadow Account Sandbox
- **Universal Broker Importer:** Ingests CSV/Excel trade logs from major brokers (Interactive Brokers, Binance, Futu, EastMoney, generic CSVs).
- **Behavioral Bias Diagnostics:** Analyzes trade distribution to mathematically flag:
  1. *Disposition Effect* (holding losers too long, selling winners prematurely)
  2. *Overtrading Syndrome* (frequency spikes following losses)
  3. *Momentum Chasing* (buying at top-percentile volume extensions)
  4. *Anchoring Bias* (hesitation to re-enter upon missed entry points)
- **Shadow Account Counterfactuals:** Simulates an idealized virtual trader executing your exact rules without human emotion, generating an 8-section PDF/HTML report displaying exactly how much PnL was left on the table.

---

## 🚀 Local Execution & Quickstart Guide

### Prerequisites
| Component | Minimum Version | Note |
|---|---|---|
| **Python** | `3.11+` | Required for LangGraph & FastAPI backend |
| **Node.js** | `18+` (LTS) | Required for React 19 / Vite frontend |
| **Docker** | `20.10+` | Optional: single-container deployment |

---

### Step 1 — Clone & Environment Setup

```bash
# Clone the repository
git clone https://github.com/Vishnummmmmmmm/lamagent.git
cd lamagent

# Set up Python virtual environment
python -m venv .venv

# Activate virtual environment
# Windows:
.venv\Scripts\activate
# Linux / macOS:
source .venv/bin/activate

# Install dependencies in editable mode
pip install -e agent/
```

Configure your environment variables:
```bash
# Copy example configuration
cp agent/.env.example agent/.env
```
Open `agent/.env` and add your preferred LLM provider API key (e.g., `OPENROUTER_API_KEY`, `OPENAI_API_KEY`, `DEEPSEEK_API_KEY`, etc.).

---

### Step 2 — Start the FastAPI Backend Orchestrator

```bash
# Start API server on port 8899
vibe-trading serve --port 8899

# Or run directly via Python
python agent/api_server.py
```
*API documentation and OpenAPI schema are live at:* `http://localhost:8899/docs`

---

### Step 3 — Launch the React 19 Sentient Dashboard

In a new terminal window:
```bash
cd frontend

# Install Node dependencies
npm install

# Start Vite development server
npm run dev
```
Open `http://localhost:5173` (or the URL displayed in your terminal) to experience the full interactive Sentient UI.

---

### Step 4 — Terminal TUI Interactive Mode (CLI)

Prefer working purely from your terminal? Run the rich interactive TUI:
```bash
# Launch interactive terminal session
vibe-trading

# Or run a single strategy prompt directly
vibe-trading run "Design a dual moving average cross strategy for NVDA with ATR stops and backtest on 2 years of daily data"
```

---

### Step 5 — Single-Command Docker Deployment

Run the complete platform (backend + compiled frontend) inside an isolated Docker container:

```bash
# Build and run with Docker Compose
docker compose up --build
```
Access the application at `http://localhost:8899`.

---

## 🔌 FastMCP Integration (Claude Desktop & Cursor)

Supreme Trading AGI ships with a native **FastMCP 2.0 stdio server** (`agent/mcp_server.py`), allowing you to use all 27 quant tools directly inside **Anthropic Claude Desktop**, **Cursor IDE**, or **Antigravity**.

### Add to Claude Desktop Configuration
Add the following to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "supreme-trading-agi": {
      "command": "vibe-trading-mcp",
      "args": [],
      "env": {
        "OPENAI_API_KEY": "your_api_key_here",
        "LANGCHAIN_PROVIDER": "openai"
      }
    }
  }
}
```

Now Claude can autonomously execute cross-market backtests, calculate Black-Scholes Greeks, recognize SMC patterns, and run Monte Carlo simulations directly inside your conversations!

---

## 🔒 Security, Sandboxing & Production Hardening

| Hardening Layer | Implementation | Security Guarantee |
|---|---|---|
| **Filesystem Containment** | `safe_path` sandbox (`src/tools/path_utils.py`) | Prevents directory traversal attacks (`../`). All file reads and writes are restricted to the active workspace. |
| **Tool Execution Sandbox** | Read-only vs. Mutating tool isolation | Read-only tools are dispatched in parallel non-blocking worker threads; mutating tools execute sequentially with strict parameter validation. |
| **API Secret Hygiene** | Multi-provider environment scoping | API keys are isolated in local `.env` files; zero client-side credential exposure via the API server. |
| **Bounded ReAct Loop** | 50-iteration safety circuit breaker | Prevents runaway LLM loops; enforces deterministic termination and automatic graceful state capture. |
| **Local-Only Admin Controls** | `127.0.0.1` binding on shutdown endpoints | Critical server lifecycle operations can only be dispatched from loopback interfaces. |

---

## 🏆 Innovation Context & Quantitative Impact Thesis

Supreme Trading AGI bridges the vast divide between institutional quantitative research and individual traders by unifying three core disciplines:

```
┌───────────────────────────────────────────────────────────────────────┐
│       ADVANCED AI           QUANTITATIVE FINANCE      SOFTWARE ENG.   │
│   • ReAct Agent Loops     • Vectorized Backtests    • FastAPI & Async │
│   • Swarm DAG Runtime     • Monte Carlo Validation  • React 19 & SSE  │
│   • Context Compression   • 4 Portfolio Optimizers  • FastMCP Server  │
│   • Autonomous Skills     • Options Greeks & Payoffs• Sandbox Storage │
└───────────────────────────────────────────────────────────────────────┘
```

### Institutional Impact Thesis
Global retail and proprietary traders lose billions annually due to **untested intuitions, emotional cognitive biases, and lack of institutional validation tools**. Elite quant firms succeed because they employ distinct teams: *data engineers, quantitative researchers, risk controllers, and execution specialists*.

**Supreme Trading AGI synthesizes this entire hedge fund structure into software.** Every trading hypothesis is adversarial debated by an AI Swarm, executed against real market data without mathematical hallucination, verified via 10,000 Monte Carlo paths, and exported in production-ready code — all in sub-second response times.

---

## 🔮 Future Development & Research Roadmap

For the comprehensive technical specification on boosting engine performance, bidirectional TradingView webhook execution, and streaming Hugging Face financial corpora, see [**FUTURE_DEVELOPMENT.md**](FUTURE_DEVELOPMENT.md):

- **⚡ Boosting the Engine:** Migrating to **Ray & Celery distributed worker meshes**, vector database acceleration (**Qdrant & Redis Semantic Cache** for <15ms responses), and 10x faster **Polars / Columnar Parquet caching**.
- **📈 Deep TradingView API Integration:** Production **FastAPI Webhook Listener** (`/api/v1/tradingview/webhook`) with HMAC authentication, automated **Pine Script v6 alert generation**, and **React 19 Advanced Charting Widgets** with SMC order block overlays.
- **🤗 Hugging Face Financial Data & Models:** Programmatic dataset streaming for **FinGPT sentiment, SEC 10-K/10-Q reports, and financial news**, alongside local offline deployment of **FinBERT** and **LoRA fine-tuned strategy coders**.
- **🔬 Frontier Quantitative R&D:** **Reinforcement Learning (PPO/SAC)** for order-slicing execution, **Nash Equilibrium game-theoretic multi-agent swarm debates**, and **High-Frequency Level 2/Level 3 Order Book Imbalance (OBI) & Cumulative Volume Delta (CVD)**.

👉 **Read the complete guide & code blueprints:** [**FUTURE_DEVELOPMENT.md**](FUTURE_DEVELOPMENT.md)

---

<div align="center">

**Hardened. Sovereign. Institutionally Validated. Ready for Scale.**

<br/>

*Developed for the Next Generation of Global Quantitative Finance.*  
© 2026 **Supreme Trading AGI (LAMAgent)**. Released under the [MIT License](LICENSE).

</div>
