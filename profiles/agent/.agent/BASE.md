# Base Engineering Rules (All Repos)

These rules are intentionally cross‑project. Project‑specific details belong in `.agent/PROJECT.md`.

## Architecture & boundaries
- Prefer explicit boundaries over convenience.
- Keep modules cohesive; avoid “god” modules.
- Dependency direction should be one‑way and explicit (e.g., Domain → Application → Adapters/IO).
- If a boundary is crossed, add a mapper or façade at the boundary.

## Model separation
- Domain models: business meaning, invariants, pure types, no IO.
- API models (DTOs): request/response payloads and validation.
- Persistence models: DB schema representations.
- Mapping is explicit, testable, and lives at boundaries.

## Error handling
- Add context as errors bubble up.
- No silent error drops.
- Prefer typed errors or error enums over stringly‑typed errors.

## Data & persistence
- Keep queries parameterized; avoid interpolation in SQL.
- Migrations are additive and reversible where feasible.
- Use repository interfaces in the domain/application layers; DB details live in adapters.

## Testing
- Unit tests for domain logic and mapping.
- Integration tests for adapters when behavior matters.
- Keep tests deterministic and isolated.

## Security
- Never commit secrets.
- Validate inputs at the boundary.
- Principle of least privilege for credentials and services.

## Observability
- Emit structured logs with stable fields.
- Add metrics or traces for critical paths.
- Correlate logs/traces per request/job.

## Documentation
- Durable architecture, boundary, runtime-policy, and long-lived product/API contract decisions → add a short decision file under `.agent/CONTEXT/decisions/` (one file per decision; see its README for the bar — most changes add none).
- Implementation notes, refactor slices, file moves, test reorganizations, and issue-local choices belong in the Issue/PR or an archive, not the active decisions log.
- Behavior changes → update docs/README/API docs as appropriate.

## Code hygiene
- Keep changes minimal and reviewable.
- Avoid “clever” code when a clear version is available.
- Prefer small, composable functions with explicit inputs/outputs.

## Technical debt discipline
- If a change touches code that violates a documented rule (`BASE.md`, `PROJECT.md`, or a decision in `.agent/CONTEXT/decisions/`), bring the touched code into compliance as part of that change — predating the rule is not an exemption.
- If you notice non‑compliant code the current change doesn't otherwise touch, don't drive‑by refactor it (that's an undisclosed scope expansion). File a follow‑up Issue describing the gap and reference it from the current PR instead, so the debt is tracked rather than silently accumulating or silently expanding scope.
