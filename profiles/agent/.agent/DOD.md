# Definition of Done

This checklist is intentionally language‑agnostic. Repos may add language‑specific sections in `.agent/PROJECT.md`.

## Must be true
- Scope matches the Issue; acceptance criteria are met.
- Implementation is covered by tests (unit/integration as appropriate).
- Lint/format/typecheck pass for the repo.
- CI is green (or explicitly justified).
- Docs updated when behavior changes.
- Before pushing: search `docs/` and `.agent/` for anything the change touches (file/module paths, service names, architecture or persistence claims) and update or correct whatever is now stale — do this every time, not only when a doc update seems obviously required. (See `.agent/PROJECT.md` for how a given repo operationalizes this check — e.g. a dedicated skill.)
- A decision file under `.agent/CONTEXT/decisions/` added or edited only when the change introduces or revises a durable architectural constraint, boundary rule, runtime policy, or long-lived product/API contract. Most changes need none; a PR that adds one says why it clears the bar in `.agent/CONTEXT/decisions/README.md`.
- Technical debt discipline (see `.agent/BASE.md`): any code this change touches that violates a documented rule has been refactored into compliance. Non-compliant code noticed but not otherwise touched by this change has a follow-up Issue filed for it, linked from the PR description — not silently left, not silently drive-by fixed.
- PR includes clear “How to test” and links the Issue (e.g., `Fixes #<id>`).
- No secrets or sensitive data were introduced.

## Status hygiene
- Issue status label reflects current state — exactly one `status:*` label, matching `.agent/WORKFLOW.md`'s lifecycle. Never `status:triage` once a branch/commit exists; never `status:review` without an open PR (or, for Spikes, a posted findings write-up).
- If blocked, the Issue notes the blocker and next step.

## Language‑specific checks (fill in per repo)
### Rust (if applicable)
- `cargo fmt --check`
- `cargo clippy --all-targets --all-features -- -D warnings`
- `cargo test --all-features`
- `cargo check --all-targets --all-features`

### Python (if applicable)
- `ruff check .`
- `ruff format --check .`
- `pytest -q`

### Node.js / TypeScript (if applicable)
- `npm run lint` or `pnpm lint`
- `npm run typecheck` (if TypeScript)
- `npm test` or `pnpm test`

### SQL / Migrations (if applicable)
- Migrations are reviewed and reversible where feasible.
- Queries are parameterized; no string interpolation.
- Schema changes documented and tested.

## Rollback note
- If runtime behavior changes, include a rollback note in the PR description or Issue.
