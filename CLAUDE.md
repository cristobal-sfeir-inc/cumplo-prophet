# cumplo-prophet

## Overview
Predictive analytics and forecasting service for Cumplo investment opportunities. **Not yet
implemented** — this is a stub (README + LICENSE + .gitignore only); no source code exists.

## Stack (intended)
Python 3.13, Poetry, FastAPI, Pydantic v2. Persistence: Firestore. Async fan-out: Pub/Sub.
Deployed on Cloud Run. Consumes `cumplo-common` for shared domain logic.

## Build & Test
No Makefile or pyproject.toml exists yet. When code is scaffolded, the standard commands will be:

- Install deps: `poetry install`
- Run tests: `poetry run pytest`
- Auto-fix lint + format: `make format`
- Verify code quality (CI gate): `make lint`
- Build Docker image: `make build`

## CI/CD
Only `.github/workflows/pr-title.yml` is active — it enforces conventional-commit format on PR
titles (e.g. `feat: Add forecasting endpoint`). No lint, test, or deploy pipeline exists yet.

On future implementation, the pattern from peer services applies: Cloud Build triggers on merge to
`master` and deploys to Cloud Run; a GitHub Actions `ci.yml` runs `make lint` on every PR.

## Git workflow
- Branch prefixes: `feat/`, `fix/`, `chore/`, `ci/`. Conventional-commit subjects.
- PR title must start with an uppercase letter after the type prefix.
- `master` is protected: every change needs a **PR + code-owner review** (`@cnsfeir-reviewer`).
  **Never push directly to `master`** — it is rejected by the ruleset.

## Gotchas
- **No code yet.** There is no Makefile, pyproject.toml, or Python source to run — this is a pure
  stub waiting for implementation.
- When scaffolding begins, pull `cumplo-common` from the private Artifact Registry PyPI. A
  `cumplo-pypi-credentials.json` SA key is required locally (gitignored); CI uses Workload Identity
  Federation instead.
- Follow the multi-stage Dockerfile and `poetry.toml` patterns from `cumplo-herald` or
  `cumplo-spotter` as the reference implementation for new Cloud Run services.
- This service is forecast/read-heavy — avoid writing back to Firestore from this service; prefer
  Pub/Sub events for any state changes.

## Before committing
After making changes, run the lint/format commands and ensure they pass; check no hardcoded
secrets — before committing.
