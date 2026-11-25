# Decisions (ADR)

A decision register, not a change log: one file per durable decision, so future work — human or agent — doesn't rediscover a boundary, silently violate one, or rebuild something already rejected.

**Scope is defined in [`.agent/PROJECT.md`](../../PROJECT.md)'s "Documentation expectations"**, which is authoritative; this README restates it operationally.

## The bar

Most changes add no decision, and that is the expected outcome. Write one only if the answer to this is yes: *if this file were deleted, would future work plausibly repeat a mistake, cross a boundary that shouldn't be crossed, or rebuild something already rejected?*

**Does not belong here:** a record that a PR happened. Implementation notes, refactor mechanics, file moves/renames, test reshaping, endpoint/response documentation, verification narratives, one-off bug stories and "what we built for issue #N" summaries belong in the Issue/PR — even for a large change. If a candidate can be summarized as "moved / split / extracted / reorganized / fixed a bug", it is not a decision.

**Subsystem mechanics go in the architecture docs** (`.agent/CONTEXT/ARCHITECTURE.md`, or wherever `PROJECT.md` says they live). How a specific driver, table or device behaves — and the traps in it — is current-state architecture, not a decision. Keep the decision here and link to the mechanics.

## The rule goes in `PROJECT.md`; the reasoning stays here

When a decision establishes something future work must follow, write the one-line imperative into the matching section of `PROJECT.md` and link this file from it; the *why* and the rejected alternatives stay here. `AGENTS.md`'s read order makes `PROJECT.md` mandatory and `.agent/CONTEXT/*` optional — a rule that lives only here will not be read before someone writes code against it.

## Files

- **One decision per file**, named `YYYY-MM-DD-<slug>.md` — the date the decision was made, a slug of its title. A new decision is a new file; it never touches another PR's lines.
- **Format:**

  ```markdown
  # <Title: the rule, stated as a short sentence>

  - **Date:** YYYY-MM-DD — when the rule was last true as stated
  - **Decision:**
  - **Why:** including the rejected alternatives
  - **Consequences:**
  ```

- **Always describes now.** When a later change supersedes a decision, **edit that file in place** so it states the current rule. Never add a second file beside the old one: a reader must not have to stitch two decisions together, and a stale one reads as current advice. Note the supersession briefly in *Why* only where it explains the rule. Keep the filename — links to it must keep working — unless its slug would then contradict the rule it holds: then rename the file to the new date and slug and update every link in the same change. Move the `Date` line only if the rule itself materially changed. A decision that no longer holds at all is deleted, along with the rule in `PROJECT.md` that points at it.
- **Reference a decision by its path** (`.agent/CONTEXT/decisions/<file>.md`), from code comments, docs and other decisions alike — never as "the 2026-08-28 entry", which stops identifying anything once two decisions share a date.
