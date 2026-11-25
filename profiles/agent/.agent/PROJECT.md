# Project Rules (Repo‑specific)

Use this file to tailor the agent to the repo. Keep it short and actionable. This is the *only* place for repo‑specific rules; everything else should remain general.

---

## Overview
- **Purpose:**
- **Primary constraints:**
- **Explicit non‑goals:**

## Architecture summary
- **High‑level modules/layers:**
  - `<module>`: purpose, boundary
- **Dependency direction:**
  - `<example: domain → app → adapters>`
- **Forbidden dependencies (if any):**
  - `<module A> must not import <module B>`

## Technology stack
- **Languages:** (e.g., Rust, Python, TS)
- **Frameworks/libraries:** (short list)
- **Database/storage:** (e.g., Postgres, Redis, S3)
- **Observability:** (logging/metrics/tracing tools)

## Model boundaries & mapping
- **Domain models live in:**
- **API DTOs live in:**
- **Persistence models live in:**
- **Mapping code lives in:**
- **Mapping tests live in:**

## Data & persistence rules
- **Migration tool + policy:**
- **Query safety:** (e.g., parameterized only)
- **Schema evolution expectations:**

## Error handling conventions
- **Error types:**
- **Where to add context:**
- **User‑facing error mapping:**

## Testing & quality gates
- **Required test types:** (unit/integration/e2e)
- **Determinism constraints:**
- **Mandatory commands:** (list repo‑specific checks)

## Security & compliance
- **Secrets management:**
- **Input validation expectations:**
- **PII/PHI handling constraints:**

## Observability requirements
- **Correlation IDs:** (format + where to attach)
- **Log fields to include:**
- **Metrics/tracing requirements:**

## API / contract rules (if applicable)
- **OpenAPI / schema location:**
- **Versioning rules:**
- **Backward compatibility expectations:**

## Performance & concurrency
- **SLAs / SLOs:**
- **Hot paths:**
- **Concurrency constraints:**

## Workflow specifics
- **Default branch:**
- **Branch naming exceptions:**
- **Review rules:**
- **Merge strategy:**

## Documentation expectations
- **Rules in this file are one line each; the reasoning goes in the linked decision file, not here.**
- **Docs that must be updated for changes:**
- **Stale-docs check:** (how this repo runs `.agent/DOD.md`'s docs search before every push — e.g. a skill in `.claude/skills/`, a script, or "by hand")
- **Decision register:** `.agent/CONTEXT/decisions/`, one file per decision (see its `README.md`); `.agent/CONTEXT/DECISIONS.md` is only a pointer stub.
- **What belongs in the decision log** (authoritative — `.agent/CONTEXT/decisions/README.md` defers to this): only a rule future work must preserve — stable layer ownership, domain/API/persistence boundary rules, contract versioning rules, cross-cutting runtime policies.
- **What does not belong:** issue-slice refactors, extracted services, renamed files, file splits, test reorganizations, endpoint/response documentation, verification narratives, cleanup passes — unless they create a reusable repo-wide rule. Most changes add no decision; a PR that adds one says why it clears the bar.
- **Promote the rule, keep the reasoning:** write the one-line imperative into the matching section of this file and put the why and rejected alternatives in a decision file, linked from the rule by path — this file is mandatory reading, `.agent/CONTEXT/*` is not.
- **Supersede in place:** a decision that changes an earlier one edits that file to state current reality; never add a second one beside it.
- **Implementation history goes in issue and PR updates;** use an archive only for notes worth keeping outside the active architectural working set.
- **Rules lifted from decisions** that have no better section above:
  - `<one-line rule>` ([decision](CONTEXT/decisions/YYYY-MM-DD-<slug>.md))

## Language‑specific conventions
### Rust
- **Crate/module placement rules:**
- **Lint configuration:**
- **SQLx usage rules:**

### Python
- **Formatting/linting:**
- **Type checking:**
- **Dependency management:**

### Node.js / TypeScript
- **Formatting/linting:**
- **Type checking:**
- **Package manager:**

---

## Repo quickstart (optional)
- **Setup:**
- **Run:**
- **Test:**
- **Common pitfalls:**
