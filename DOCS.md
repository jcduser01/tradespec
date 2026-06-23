# Documentation index

A routing map from each document to the part of the project it is the source of
truth for. Before completing a change, find the row(s) whose subject your change
touched and update those documents **in the same change** — stale docs are defects.
When you add or remove a document, update this index too.

| Document | Source of truth for |
|---|---|
| [README.md](README.md) | Project overview, monorepo layout, tech stack, environment variables, local run commands, quality check commands, shared domain vocabulary, and current MVP status |
| [USER_GUIDE.md](USER_GUIDE.md) | End-user workflow — pre-trade evaluation, trade building, validation, position sizing, active trade monitoring, journaling, and pattern review |
| [apps/api/README.md](apps/api/README.md) | FastAPI backend — API routes, request/response contracts, how to run and test the backend service |
