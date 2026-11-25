# Template Repository Guide

This repository is the template source consumed by [`repokit-cli`](https://github.com/mrcrgl/repokit).

Use this guide if you need to understand how the template repository is organized, what each profile owns, or how changes here affect target repositories.

## Relationship to `repokit-cli`

`repokit-cli` applies selected profiles from this repository into a target repository, records template metadata in `.kit.toml`, and later updates the target repo using the same template source.

In practice, that means this repository owns:

- the files that should appear in target repositories
- merge behavior for files that should be updated rather than overwritten blindly
- optional GitHub label definitions that can be synced into target repositories

`repokit-cli` owns:

- fetching the template source
- applying selected profiles in order
- tracking template state in `.kit.toml`
- performing update, status, and config operations in target repositories

## Profile structure

`repokit-cli` expects a template repository shaped like this:

```text
profiles/
  base/
    .repokit/
  <profile>/
    .repokit/
```

Rules for this repository:

- `profiles/base/` is required and is always applied first.
- Additional profiles are optional overlays.
- If two selected profiles provide the same path, the later profile wins unless a merge rule applies.

Current profiles in this repository:

- `base`
  Provides shared repository defaults such as issue templates, pull request template, base `.gitignore`, label definitions, and merge rules.
- `agent`
  Provides reusable `.agent/` guidance plus `AGENTS.md` for agent-driven repositories.
- `claude`
  Adds a root `CLAUDE.md` that imports `AGENTS.md` for Claude Code. Select it together with the `agent` profile.
- `python`
  Adds Python-specific ignore patterns and `.agent/LANGUAGE_RULES.md` content.
- `rust`
  Adds Rust-specific ignore patterns and `.agent/LANGUAGE_RULES.md` content.

The `agent` profile's decision register is `.agent/CONTEXT/decisions/`: one file per durable architecture rule, not an issue log. `.agent/CONTEXT/decisions/README.md` defines the bar and the file format; `.agent/CONTEXT/DECISIONS.md` is only a pointer stub.

## `repokit` configuration inside profiles

The `.repokit/` directory is template metadata for `repokit-cli`. It is not copied into target repositories.

This repository currently uses:

- `profiles/base/.repokit/merge.toml`
  Merges `.gitignore` using the `gitignore` strategy.
- `profiles/base/.repokit/github/labels.toml`
  Defines the GitHub labels that can be added to target repositories.
- `profiles/agent/.repokit/merge.toml`
  Merges `.agent/LANGUAGE_RULES.md` as a Markdown block so language-specific overlays can extend it cleanly.
  It deliberately has no rule for the decision register; its comments explain why a plain sync already keeps a repo's decision files and `DECISIONS.md` intact.

## Migrating to one file per decision

Repositories synced before the `agent` profile split the decision register keep all their decisions in a single `.agent/CONTEXT/DECISIONS.md`. On the next `repokit update`:

- `.agent/CONTEXT/decisions/README.md` is added.
- If `DECISIONS.md` was never edited, it is replaced by the stub. It held no entries, so nothing is lost.
- If `DECISIONS.md` holds entries, it is kept and the stub is written to `.agent/CONTEXT/DECISIONS.md.new`. The other changed agent files may get `.new` files as well; merge them as usual.

To finish the migration, in one pull request:

1. Split every entry into `.agent/CONTEXT/decisions/YYYY-MM-DD-<slug>.md`, using the format in `decisions/README.md`: the title states the rule, the date is the entry's date, and the reasoning keeps any rejected alternatives. Move every entry, even ones that no longer seem to clear the bar, so the migration can be reviewed as lossless. Delete or rewrite them in a separate change.
2. Lift the rules into `.agent/PROJECT.md`: for each decision that future work must follow, write the one-line imperative into the matching section and link the decision file by path. A rule that lives only in the register is not read before code is written, because `AGENTS.md` makes `PROJECT.md` mandatory reading and `.agent/CONTEXT/*` optional.
3. Replace `DECISIONS.md` with the stub (`mv .agent/CONTEXT/DECISIONS.md.new .agent/CONTEXT/DECISIONS.md`). Leave it in place rather than deleting it: a missing file would be recreated by the next sync.
4. Search the repository for `DECISIONS.md` and update references to point at `decisions/` or at the specific decision file.

After that, `DECISIONS.md` matches the template, and later syncs leave it and the decision files untouched.

## Authoring and maintenance guidance

When changing this repository:

- keep profiles focused on a single concern
- put shared defaults in `base`
- add language or workflow-specific behavior in overlays
- update docs when profile responsibilities or public usage expectations change
- verify that the documented structure still matches the actual files under `profiles/`

For repository-local contributor workflow, see [AGENTS.md](../AGENTS.md).
