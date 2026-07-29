# Decisions (ADR)

This file is the active architecture decision register.
Use it only for durable rules that should shape how future software is architected, written, and reviewed.

Keep an entry only when it establishes a long-lived constraint such as:
- boundary ownership
- dependency direction
- domain / API / persistence separation
- contract and versioning rules
- runtime or operational policies
- repository-wide coding standards

Do not add issue-scoped implementation notes, refactor mechanics, file moves, test reshaping, cleanup passes, or other work-log entries unless they create a reusable architectural rule future work must follow.

If the decision would stop mattering once the current implementation is rewritten, it does not belong here.
If the entry can be summarized as "moved", "split", "extracted", or "reorganized", it probably belongs in the Issue, PR, or archive instead.
Prefer one entry per reusable rule, written as a normative statement.

Format:
- Date (YYYY-MM-DD)
- Decision
- Why
- Consequences
