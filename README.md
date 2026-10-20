# GBLOXK Freight AI — Self-Aware Freight Operating System

> **GBLOXK Freight AI (Self-Aware Freight Operating System) — the AI self-aware freight operating system.** — A product of **GBLOXKOS LLC**

A multi-million-dollar-grade AI-native freight management platform with **real live data only** (no simulated/fake data), **5-layer anti-hallucination guardrails**, **self-healing**, **self-learning**, and **self-awareness** capabilities.

Built by **GBLOXKOS LLC** — not affiliated with Alibaba Cloud, Flexport, Uber Freight, Convoy, or any other company.

---

## Table of Contents

- [Quick Start](#-quick-start)
- [What Is GBLOXK Freight AI?](#-what-is-gbloxk-freight-ai)
- [Architecture](#-architecture)
- [AI Agent Fleet](#-ai-agent-fleet-7-agents)
- [Anti-Hallucination Protocol](#-anti-hallucination-protocol-5-layers)
- [Real Live Data Only](#-real-live-data-only)
- [Project Structure](#-project-structure)
- [API Endpoints](#-api-endpoints)
- [Authentication](#-authentication)
- [Environment Variables](#-environment-variables)
- [Scripts](#-management-scripts)
- [Docker Deployment](#-docker-deployment)
- [Differentiators](#-differentiators)
- [Developer](#-developer)
- [License](#-license)

---

## 🚀 Quick Start

### Prerequisites

- **Python 3.11+** (backend)
- **Node.js 20+** (frontend)
- **Ollama** (for local AI models)
- **Docker + Docker Compose** (for production / infra services: PostgreSQL, Redis, ChromaDB)

### Local Development (Fastest)

```bash
# 1. Install Ollama (if not already installed)
curl -fsSL https://ollama.com/install.sh | sh

# 2. Pull recommended AI models
ollama pull qwen2.5:3b
ollama pull mxbai-embed-large:335m

# 3. Start the stack
./start.sh
```

**Access:**
- 🌐 **Dashboard:** http://localhost:5173
- 🔧 **API Docs:** http://localhost:8000/docs
- 🧠 **AI Health:** http://localhost:8000/api/v1/ai/health

> No account required — click **"Continue to Dashboard"** on the login screen to enter demo mode instantly.

### Full Production Deployment (Docker)

```bash
# One-time full install: Ollama models, Docker infra, Python deps, DB init, frontend build
./install.sh

# Then access via:
#   http://localhost:5173 (frontend)
#   http://localhost:8000 (backend API)
#   http://localhost (via Caddy TLS reverse proxy, production)
```

See **[INSTALL.md](./INSTALL.md)** for the complete installation and setup guide.

See **[USER_GUIDE.md](./USER_GUIDE.md)** for the full user guide.

---

## 📖 What Is GBLOXK Freight AI?

**GBLOXK Freight AI** (a product of **GBLOXKOS LLC**) is not just another TMS or digital freight marketplace. It is a **self-aware, self-healing, self-learning AI operating system** for freight — the intelligence layer that turns a fleet of any size (Sprinter vans to semi-trucks) into an autonomous, profit-maximizing machine.

### Core Capabilities

| Capability | Description |
|---|---|
| **True Profitability Engine** | Full cost stack (fuel, tolls, driver pay, deadhead, accessorials) + risk assessment + opportunity cost |
| **Agentic AI** | 7 autonomous agents continuously evaluate loads, match vehicles, optimize routes, automate dispatch, detect fraud |
| **Anti-Hallucination Guardrails** | 5-layer verification protocol with Truth Scores on every AI decision |
| **Real Road Routing** | OSRM real driving distance and ETA — never straight-line estimates |
| **Live Fuel Prices** | EIA real-time diesel prices when API key is configured |
| **Self-Healing** | Detects and recovers from model/system degradation with automatic rollback |
| **Self-Learning** | Closed-loop RL: decision→outcome→reward→model improvement |
| **Self-Awareness** | Every AI prediction carries calibrated confidence; agents know what they don't know |
| **Digital Twin** | Simulate "what-if" scenarios before committing to changes |
| **Fraud Shield** | AI-powered carrier, driver, and load vetting |
| **Local-First Privacy** | Runs on your machine with private Ollama LLMs — no data leaves your system |
| **Zero Fake Data** | Missing data returns `null` — never fabricated values |

---

## 🏗️ Architecture

```
                    ┌──────────────────────────────────────────────────┐
                    │          Frontend (React 18 + TS)               │
                    │  Dashboard • Load Board • Fleet • Routes       │
                    │  Dispatch • Analytics • AI Center • Fraud      │
                    └────────────────────────┬───────────────────────┘
                                             │ REST + WebSocket
                    ┌────────────────────────▼───────────────────────┐
                    │          Backend (FastAPI + Python 3.11)        │
                    │  Loads • Vehicles • Routes • Dispatch • AI     │
                    │  Fraud • Auth • WebSocket Real-time            │
                    └──────┬───────────┬───────────┬─────────────────┘
                           │           │           │
              ┌────────────▼──┐  ┌───▼──────┐  ┌────▼──────────────────┐
              │  SQLite (dev) │  │  Redis  │  │  Ollama (local LLMs)   │
              │  PostgreSQL   │  │ (cache/ │  │  qwen2.5:3b            │
              │  (prod)      │  │  queue)  │  │  mxbai-embed:335m      │
              └───────────────┘  └─────────┘  └────────────────────────┘
                          │           │           │
                   Celery Workers (background: health, healing, learning)
```

### Tech Stack

| Layer | Technology |
|---|---|
| **Backend** | Python 3.11, FastAPI, SQLAlchemy 2.0 (async), Pydantic v2 |
| **Frontend** | React 18, TypeScript, Vite 5, Tailwind CSS 3, Zustand, Recharts, Lucide React |
| **Database** | SQLite (dev) / PostgreSQL 16 (prod) |
| **AI Engine** | Ollama (local LLMs — no cloud dependency) |
| **Real Data** | OSRM (road routing), Nominatim/OpenStreetMap (geocoding), EIA (diesel prices) |
| **Background Jobs** | Celery + Redis |
| **Real-time** | Socket.IO over ASGI |
| **Reverse Proxy** | Caddy (TLS, in production) / Vite proxy (dev) |
| **Containerization** | Docker + Docker Compose |

> **Developed by GBLOXKOS LLC** — a freight technology company building AI-native operating systems for the trucking industry.

---

## 🤖 AI Agent Fleet (7 Agents)

GBLOXK Freight AI runs a **swarm of 7 AI agents**. Every agent is built on the `BaseAgent` base class with health tracking, decision logging, and self-awareness capabilities.

| # | Agent | Name | Purpose |
|---|-------|------|---------|
| 1 | **Guardian** | `guardian` | 5-layer anti-hallucination verification protocol on every AI decision |
| 2 | **Load Evaluator** | `load_evaluator` | True profitability: full cost stack + risk + confidence, Guardian-verified |
| 3 | **Fraud Shield** | `fraud_shield` | Load/driver vetting, scam detection, risk scoring |
| 4 | **Digital Twin** | `digital_twin` | Simulate-before-execute "what-if" scenarios on the real network |
| 5 | **Self-Awareness** | `self_awareness` | Tracks agent accuracy, confidence calibration, degradation detection |
| 6 | **Self-Healing** | `self_healing` | System health monitoring, auto-recovery, circuit breakers, model rollback |
| 7 | **Self-Learning** | `self_learning` | Closed-loop RL: captures decisions → outcomes → rewards → recalibration |

**Agent lifecycle on every decision:**
1. Agent receives context + injects real grounding data into the prompt
2. LLM (via Ollama) produces a structured JSON response
3. Response passes through the Guardian's 5-layer verification
4. Truth Score is computed and persisted
5. Decision is logged with confidence, verification results, and audit trail
6. Outcome is captured; reward is computed for self-learning

---

## 🛡️ Anti-Hallucination Protocol (5 Layers)

In freight, a hallucinated rate, route, or ETA costs real money. GBLOXK Freight AI ships a **5-layer anti-hallucination stack** on every AI decision. This is the core differentiator built by **GBLOXKOS LLC**.

### Layer 0 — Grounding
Every LLM call is **grounded** with real current system data (loads, vehicles, fuel prices, OSRM distances) injected into the prompt. The model never invents facts from memory. Structured JSON output schema is enforced.

### Layer 1 — Constraint Validation
After the LLM produces a decision, a **rule-based validator** checks hard constraints:
- Weight ≤ vehicle `max_weight_lbs`
- Volume ≤ vehicle `max_volume_cuft`
- Dimensions ≤ vehicle capacity
- Equipment type compatible
- Hazmat ≤ driver endorsements
- HOS (hours of service) not exceeded

If any constraint fails → decision rejected, model flagged.

### Layer 2 — Cross-Reference
- **Numbers vs tables:** AI-calculated profit/mile is re-derived with the deterministic cost engine (`cost_engine.py`). If deviation > 2% tolerance (`PROFIT_DEVIATION_TOLERANCE`), it's flagged.
- **Consistency:** AI's stated ETA cross-checked against route distance/speed computation.

### Layer 3 — Uncertainty Calibration
Each prediction emits raw confidence (0–1). A calibrated model converts raw confidence → true probability using historical accuracy data (isotonic/platt scaling). If calibrated probability < `CONFIDENCE_THRESHOLD` (default 0.85), the agent must defer to a deterministic fallback or human.

### Layer 4 — Oracle-Critic Debate
A second, independent model instance (the "Critic") reviews the first model's (the "Oracle's") reasoning. Checks for: internal contradictions, invented facts, math errors, constraint violations, unsupported rate figures. Only if the Critic endorses does it pass; otherwise it loops up to `MAX_CRITIC_ATTEMPTS` (default 3), then escalates.

### Layer 5 — Human Escalation + Audit Log
High-stakes actions (booking a load, spending money, committing capacity) always route through an approval gate. Every AI decision, its confidence, verification results, and escalations are **audit-logged** (immutable) in the `truth_results` table.

**Result:** A **Truth Score** (0.0–1.0) on every decision, displayed in the AI Control Center.

```
┌─────────────────────────────────────────────────┐
│  AI Decision                                     │
├─────────────────────────────────────────────────┤
│  L0 Grounding      ✅ all required fields       │
│  L1 Constraints    ✅ weight, volume, equipment  │
│  L2 Cross-Ref      ✅ profit matches cost engine  │
│  L3 Calibration    ✅ 0.92 → 0.89 (pass ≥0.85)    │
│  L4 Critic         ✅ endorsed                    │
│  ────────────────────────────────────────────────│
│  Truth Score: 0.91  → VERIFIED                   │
└─────────────────────────────────────────────────┘
```

---

## 🌍 Real Live Data Only

GBLOXK Freight AI uses **ONLY real, live data**. There is zero simulated or fabricated data anywhere in the system.

| Source | Type | Fallback When Unavailable |
|--------|------|--------------------------|
| **OSRM** | Real road distance & ETA | `null` (never an estimate) |
| **Nominatim/OSM** | Real geocoding | `null` (location unknown) |
| **EIA API** | Live US diesel prices | `null` (must add free API key) |
| **Ollama** | Local LLMs (qwen2.5:3b, mxbai-embed) | Deterministic cost engine |
| **Your data** | Loads, vehicles, drivers, rates | Real cost stack from user input |

**No fake data ever.** When a required input is missing (e.g., no EIA API key), the system:
1. Explicitly reports data as `null` / "unavailable"
2. Excludes that cost component rather than fabricating a value
3. Shows `—` in the UI where a number cannot be computed

---

## 📁 Project Structure

```
flexoport_AI/
├── SPEC.md                        # Full technical specification
├── INSTALL.md                     # Installation & setup guide
├── USER_GUIDE.md                  # Full user manual
├── README.md                      # This file
├── docker-compose.yml             # Production Docker deployment (8 services)
├── Caddyfile                      # TLS reverse proxy config
├── .env                           # Environment configuration (dev mode)
├── install.sh                     # One-time install & setup script
├── start.sh                       # Daily startup script
├── restart.sh                     # Quick backend restart
├── stop.sh                        # Safe shutdown script
├── backend.log                    # Backend runtime log
├── frontend.log                   # Frontend runtime log
├── gbloxr_freight.db                   # SQLite database (dev)
│
├── backend/
│   ├── Dockerfile                 # Python 3.11-slim container
│   ├── requirements.txt           # 25 pinned Python dependencies
│   ├── init.sql                   # PostgreSQL initialization
│   └── app/
│       ├── main.py                # FastAPI entry point (hardened)
│       ├── core/
│       │   ├── config.py          # Pydantic settings (multi-env resolution)
│       │   ├── database.py        # Async DB engine + session + migrations
│       │   ├── security.py        # JWT, bcrypt, auth utilities
│       │   └── websocket_manager.py
│       ├── models/                # SQLAlchemy 2.0 async models (20+ tables)
│       │   ├── company.py         # Company, User
│       │   ├── vehicle.py         # Vehicle, Trailer
│       │   ├── driver.py          # Driver (HOS, endorsements, scores)
│       │   ├── load.py            # Load (origin/dest, cargo, finance, AI results)
│       │   ├── route.py           # Route, Dispatch
│       │   ├── intelligence.py    # AI decisions, rewards, agent health, truth, fraud
│       │   └── financials.py      # RateCon, Settlement, Invoice, CostEntry
│       ├── schemas/               # Pydantic v2 schemas
│       ├── api/
│       │   ├── deps.py            # Shared dependencies
│       │   └── v1/                # REST endpoints (auth, loads, vehicles, drivers,
│       │       │                  #   routes, analytics, ai, fraud)
│       ├── services/              # Business logic
│       │   ├── cost_engine.py     # Deterministic ground-truth cost stack
│       │   ├── routing_service.py # OSRM real road routing
│       │   ├── route_service.py   # AI-optimized routing
│       │   ├── dispatch_service.py
│       │   ├── fuel_service.py    # EIA live diesel prices
│       │   ├── geocode_service.py # Nominatim geocoding
│       │   ├── load_service.py    # Load CRUD + AI evaluation
│       │   ├── vehicle_service.py # Fleet + AI matching
│       │   ├── driver_service.py
│       │   └── analytics_service.py
│       ├── ai/
│       │   ├── ollama_client.py   # Ollama HTTP client (retries)
│       │   ├── expert.py          # Super AI Expert (KB + LLM + anti-hall)
│       │   ├── antihallucination.py # 7-layer anti-hallucination verification
│       │   ├── calibration.py     # Confidence calibration
│       │   ├── digital_twin.py    # Digital Twin + Self-Awareness
│       │   ├── learning_loop.py   # Self-Learning RL loop
│       │   └── agents/            # 7 registered agents
│       │       ├── base_agent.py
│       │       ├── guardian_agent.py
│       │       ├── load_evaluator.py
│       │       ├── fraud_shield.py
│       │       ├── self_healing.py
│       │       └── __init__.py    # Agent registry
│       └── workers/
│           ├── celery_app.py      # Celery config + beat schedule
│           └── tasks.py           # Health check, self-healing, calibration, learning
│
├── frontend/
│   ├── Dockerfile                 # Multi-stage: node builder → nginx
│   ├── package.json               # npm config (React, TypeScript, Vite, Tailwind)
│   ├── vite.config.ts             # Vite config (API proxy, code splitting)
│   ├── tailwind.config.js         # Tailwind + custom 'flex' color palette
│   ├── tsconfig.json
│   ├── nginx.conf                 # nginx SPA config with reverse proxy
│   ├── src/
│   │   ├── main.tsx               # React entry (BrowserRouter, ErrorBoundary)
│   │   ├── App.tsx                # Route definitions (10 pages)
│   │   ├── styles/globals.css     # Tailwind + custom keyframes
│   │   ├── services/api.ts        # API client (hardened, safe fallbacks)
│   │   ├── store/index.ts         # Zustand stores (auth, UI, loads, fleet, analytics)
│   │   ├── types/index.ts         # TypeScript interfaces
│   │   ├── utils/formatters.ts    # Currency/number/date formatters
│   │   ├── hooks/
│   │   │   ├── useWebSocket.ts    # WebSocket hook (exponential backoff)
│   │   ├── components/
│   │   │   ├── layout/            # Layout, Sidebar, TopBar
│   │   │   └── ui/                # Button, Card, Input, Select, Badge, Modal, etc.
│   │   └── pages/                 # 10 page components
│   │       ├── Login.tsx          # Demo access + auth
│   │       ├── Dashboard.tsx      # KPI overview, fleet, recent loads, agent health
│   │       ├── LoadBoard.tsx      # Load CRUD + AI evaluation + matching
│   │       ├── Fleet.tsx          # Vehicle management + specs
│   │       ├── Routes.tsx         # Route optimization with cost breakdown
│   │       ├── Dispatch.tsx       # Kanban dispatch board + check-calls
│   │       ├── Analytics.tsx      # Profitability, trends, deadhead, charts
│   │       ├── AIControlCenter.tsx# Agent status, chat, digital twin, guardian log
│   │       ├── AISuperExpert.tsx  # Chatbot — answers questions about GBLOXK Freight AI
│   │       ├── FraudShield.tsx    # Load/driver vetting + fraud signals
│   │       └── Settings.tsx       # Account, AI models, integrations, database
│   └── dist/                      # Pre-built production output
└── frontend.log
```

---

## 🔌 API Endpoints

All endpoints are under `/api/v1/`. The `company_id` query parameter (default: `1`) scopes data to your tenant.

### Auth (`/auth`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/login` | Login with email/password, returns JWT token |
| POST | `/auth/register` | Register new user + company |

### Loads (`/loads`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/loads` | List loads (filter: `?status=available`) |
| POST | `/loads` | Create a load (auto-geocoded, auto-distance via OSRM) |
| GET | `/loads/{id}` | Get a single load |
| PUT | `/loads/{id}` | Update a load |
| DELETE | `/loads/{id}` | Delete a load |
| POST | `/loads/{id}/evaluate` | **AI profitability evaluation** (Guardian-verified) |
| POST | `/loads/{id}/match` | **AI vehicle matching** — ranks fleet for the load |

### Vehicles (`/vehicles`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/vehicles` | List fleet vehicles |
| POST | `/vehicles` | Create a vehicle |
| PUT | `/vehicles/{id}` | Update a vehicle |
| DELETE | `/vehicles/{id}` | Delete a vehicle |

### Drivers (`/drivers`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/drivers` | List drivers |
| POST | `/drivers` | Create a driver |
| PUT | `/drivers/{id}` | Update a driver |
| DELETE | `/drivers/{id}` | Delete a driver |

### Routes & Dispatch
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/routes/optimize` | **Optimize route** for a load (OSRM + cost stack + carbon) |
| POST | `/dispatches` | **Create dispatch** (auto-generates AI instructions + check-calls) |
| GET | `/dispatches` | List dispatches (filter: `?status=delivered`) |
| PATCH | `/dispatches/{id}` | Update status or reassign vehicle/driver |

### Analytics (`/analytics`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/analytics/kpis` | KPI summary (revenue, profit, deadhead %, utilization, truth score) |
| GET | `/analytics/profitability` | Profitability by dimension (`?dimension=lane\|vehicle\|driver`) |
| GET | `/analytics/deadhead` | Deadhead analysis + reduction suggestions |
| GET | `/analytics/fuel?state=TX` | Live diesel price (EIA) by state |
| GET | `/analytics/trend` | Monthly revenue vs cost |
| GET | `/analytics/load-status` | Load status distribution |

### AI (`/ai`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/ai/ask` | **Co-pilot chat** — grounded, anti-hallucination verified |
| GET | `/ai/agents/status` | All 7 agent health states |
| GET | `/ai/learning/progress` | RL accuracy progress (30d/90d) |
| GET | `/ai/self-awareness/{agent_type}` | Agent capability assessment |
| POST | `/ai/simulate` | **Digital Twin** what-if simulation |
| GET | `/ai/guardian/log` | Anti-hallucination Truth Score audit log |
| GET | `/ai/health` | Ollama health + available models |
| GET | `/ai/models` | List Ollama models |
| POST | `/ai/models/pull` | Pull a new Ollama model (allowlisted) |
| POST | `/ai/expert` | **Super AI Expert** — answers questions about GBLOXK Freight AI |

### Fraud (`/fraud`)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/fraud/vet/load` | Vet a load for fraud (ZIP/rate/distance checks) |
| POST | `/fraud/vet/driver/{id}` | Vet a driver (license, medical card) |
| GET | `/fraud/signals` | List fraud signals |

### Health
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health/live` | Liveness probe |
| GET | `/health/ready` | Readiness probe (DB + Ollama check) |
| GET | `/` | App info root |

---

## 🔐 Authentication

GBLOXK Freight AI supports optional JWT-based authentication. In dev mode, auth is optional — you can use the app without logging in (all data is scoped to `company_id=1`).

```bash
# Login
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"user@company.com","password":"secret"}'

# Use token
curl -H "Authorization: Bearer <token>" \
  http://localhost:8000/api/v1/loads?company_id=1
```

---

## 📝 Environment Variables (.env)

```bash
# Core
APP_NAME=GBLOXK Freight AI
APP_VERSION=1.0.0
DEBUG=true

# Database (SQLite for dev — no external dependencies)
DATABASE_URL=sqlite+aiosqlite:///./gbloxr_freight.db
REDIS_URL=redis://localhost:6379/0

# Ollama (local LLMs — no cloud dependency)
OLLAMA_HOST=http://localhost:11434
OLLAMA_PRIMARY_MODEL=qwen2.5:3b
OLLAMA_SECONDARY_MODEL=feadxus/Llama-3.2-3B-Instruct-abliterated:Q4_K_S
OLLAMA_CRITIC_MODEL=qwen2.5:3b
OLLAMA_EMBED_MODEL=mxbai-embed-large:335m

# ChromaDB (vector store — optional)
CHROMA_HOST=localhost
CHROMA_PORT=8500

# Security
JWT_SECRET=dev-secret-change-in-production
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=720

# AI Guardrails
CONFIDENCE_THRESHOLD=0.85
MAX_CRITIC_ATTEMPTS=3
PROFIT_DEVIATION_TOLERANCE=0.02

# Live Data APIs (optional — leave blank for fully offline mode)
EIA_API_KEY=           # Free: https://www.eia.gov/opendata/
DAT_API_KEY=           # Requires DAT subscription
TRUCKSTOP_API_KEY=     # Requires Truckstop.com subscription
MAPBOX_TOKEN=          # Optional: Mapbox for geocoding/routing
```

---

## ⚙️ Management Scripts

| Script | Action |
|--------|--------|
| `./install.sh` | Complete first-time setup (Ollama check, model pull, Docker infra, Python deps, DB init, frontend build, health check) |
| `./start.sh` | Daily startup: backend (uvicorn `:8000`) + frontend dev server (Vite `:5173`) |
| `./restart.sh` | Quick backend-only restart |
| `./stop.sh` | Safe shutdown of all GBLOXK Freight AI processes (supports `--force` and `--quiet`) |

---

## 🐳 Docker Deployment

Production deployment with Docker Compose:

```bash
# Start all services (PostgreSQL, Redis, Ollama, ChromaDB, Backend, Celery, Frontend, Caddy)
docker compose up -d

# View logs
docker compose logs -f backend
docker compose logs -f frontend

# Scale workers
docker compose up -d --scale celery-worker=4

# Stop everything
docker compose down
```

**Services (8 total):**

| Service | Port | Purpose |
|---------|------|---------|
| `postgres` | 5432 | PostgreSQL 16 database |
| `redis` | 6379 | Redis cache + Celery broker |
| `ollama` | 11434 | Local LLM inference |
| `chromadb` | 8500 | Vector store |
| `backend` | 8000 | FastAPI API server |
| `celery-worker` | — | Background task processing |
| `celery-beat` | — | Scheduled tasks (health, learning) |
| `frontend` | 5173 | nginx serving built React SPA |
| `caddy` | 80/443 | TLS reverse proxy |

---

## 🏆 Differentiators

| Feature | GBLOXK Freight AI (by GBLOXKOS LLC) | Traditional TMS / Others |
|---------|------------------------------|--------------------------|
| Real OSRM routing | ✅ | ❌ (straight-line estimates) |
| Live EIA fuel prices | ✅ | ❌ (hardcoded averages) |
| Anti-hallucination (5-layer) | ✅ | ❌ |
| Self-healing + rollback | ✅ | ❌ |
| Self-learning RL loop | ✅ | Partial |
| Self-awareness / calibration | ✅ | ❌ |
| Digital Twin simulation | ✅ | ❌ |
| Local-first (Ollama) | ✅ | ❌ (cloud LLM dependency) |
| No fake data ever | ✅ | ❌ (invented data common) |
| 1-truck to 10,000-truck | ✅ | Enterprise-only |
| Built-in fraud shield | ✅ | ❌ / add-on |
| Carbon/ESG accounting | ✅ | ❌ |
| Offline mode | ✅ | ❌ |

---

## 👨‍💻 Developer

**GBLOXK Freight AI is developed by GBLOXKOS LLC** — a freight technology company building AI-native operating systems for the trucking industry.

- **Company:** GBLOXKOS LLC
- **Product:** GBLOXK Freight AI — Self-Aware Freight Operating System
- **Note:** GBLOXK Freight AI is **NOT** affiliated with Alibaba Cloud, Flexport, Uber Freight, Convoy, or any other company. The name "GBLOXK Freight AI" is independently coined by GBLOXKOS LLC.

---

## 📄 License

Proprietary — **GBLOXKOS LLC**

---

## Related Documentation

- **[INSTALL.md](./INSTALL.md)** — Complete installation, setup, and configuration guide
- **[USER_GUIDE.md](./USER_GUIDE.md)** — Full user manual for all 10 dashboard pages
- **[SPEC.md](./SPEC.md)** — Full technical specification and vision document

---

> **Built for the real world. No simulations. No hallucinations. Just profit.**
> — GBLOXKOS LLC