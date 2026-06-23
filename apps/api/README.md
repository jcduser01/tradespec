# TradeSpec API

FastAPI backend for the TradeSpec MVP.

All routes are prefixed with `/api`. The interactive docs are available at
`http://localhost:8000/docs` when the server is running.

## Running the server

```bash
cd apps/api
python -m pip install -e ".[dev]"
python -m uvicorn app.main:app --reload --port 8000
```

Health check:

```bash
curl http://localhost:8000/api/health
```

## Running tests and linting

```bash
cd apps/api
python -m pytest
python -m ruff check .
```

## Routes

| Method | Path | Description |
|---|---|---|
| GET | `/api/health` | Service health and environment |
| GET | `/api/market-snapshot/{ticker}` | Raw market snapshot for a ticker (Yahoo Finance) |
| POST | `/api/evaluate-ticker` | Pre-trade rule evaluation (`VALID` / `WAIT` / `INVALID`) |
| POST | `/api/validate-trade` | Trade plan validation with hard and soft checks |
| POST | `/api/trade-specs` | Save an approved trade plan to SQLite |
| POST | `/api/position-size` | Deterministic position sizing from account size and risk % |
| GET | `/api/active-trades` | Active trade list with guidance (`HOLD` / `EXIT` / `EXPIRED`) |
| POST | `/api/journal-entry` | Create a journal entry (triggers AI observation stub) |
| GET | `/api/journal-entries` | List all journal entries |
| GET | `/api/journal-summary` | Pattern summary across closed trades |

## Environment variables

Create `apps/api/.env` from `apps/api/.env.example`.

| Variable | Default | Description |
|---|---|---|
| `TRADESPEC_APP_NAME` | `TradeSpec API` | API application name |
| `TRADESPEC_ENVIRONMENT` | `development` | `development`, `test`, or `production` |
| `TRADESPEC_ALLOWED_ORIGINS` | `http://localhost:3000` | Comma-separated frontend origins for CORS |
| `TRADESPEC_SQLITE_URL` | `sqlite:///./tradespec.db` | SQLite connection string |
| `TRADESPEC_MARKET_DATA_PROVIDER` | `yahoo-finance` | Market data backend |
| `TRADESPEC_MARKET_DATA_CACHE_TTL_SECONDS` | `300` | In-process snapshot cache lifetime in seconds |
| `TRADESPEC_AI_PROVIDER` | `stub` | AI backend (`stub` is the only supported value for now) |

## Source layout

```text
app/
  main.py                  — application factory
  api/
    router.py              — top-level APIRouter that assembles all route modules
    routes/                — one module per feature area
  core/
    config.py              — pydantic-settings Settings class
    dependencies.py        — FastAPI dependency providers
  domain/
    active_trades/         — active trade stabilizer (HOLD / EXIT / EXPIRED guidance)
    ai_services/           — AI service protocol and stub implementation
    evaluations/           — pre-trade evaluation models and service
    journal/               — journal entry models, repository, service, and pattern summary
    market_data/           — Yahoo Finance adapter, normalizer, cache, and error types
    position_sizing/       — deterministic position sizing
    rules/                 — pre-trade rule engine and EvaluationStatus enum
    trade_specs/           — trade plan models, validation, and SQLite repository
    glossary.py            — shared domain vocabulary
  persistence/
    models.py              — SQLAlchemy table definitions
```
