# Workflow 

GitHub Repository: {{REPOSITORY_NAME}}
This workflow is intentionally general so it can be reused across repositories.

## Source of truth
- Every change maps to exactly one GitHub Issue.
- The Issue contains current status via labels and/or the Issue form status field.
- If scope changes, update the Issue (plan + acceptance criteria).
- Progress notes, refactor rationale, temporary trade-offs, and issue-scoped execution details belong in the Issue or PR, not in `.agent/CONTEXT/decisions/`.

## Issue types
Use the repo’s issue templates:
- Story: user-facing change or feature.
- Task: internal work or a unit of delivery.
- Bug: defect or regression.
- Docs: documentation updates.
- Spike: time-boxed research to reduce uncertainty.

## Status tracking (labels)
- Every open Issue must carry exactly one `status:*` label at all times — never zero, never more than one. An open Issue with no `status:*` label is a bug in itself; apply `status:triage` immediately if you find one.
- Lifecycle: `status:triage` → `status:in-progress` → `status:review` → `status:done`.
- **Mandatory**: the moment work starts on an Issue — i.e. at branch creation (see Branching, below), before the first commit — swap its label to `status:in-progress` in the same action. This is not optional and not something to defer until "later in the session." An Issue must never sit at `status:triage` while a branch or commits for it already exist.
- Moving to `status:review` requires an actual PR open against it (or, for Spikes with no code change, a findings write-up posted to the Issue). Don't set `status:review` preemptively.
- If blocked, use `status:blocked` and comment with the blocker + next step.
- Before ending any session that touched an Issue, verify its label still matches reality (see `.agent/DOD.md` Status hygiene).

## Branching (GitHub Flow)
- Always run `git fetch origin` before creating any branch.
- Always create a local branch from the primary branch (`main` or `master`, whichever exists).
- Create the branch as `feat/<id>-<slug>`, `fix/<id>-<slug>`, or `chore/<id>-<slug>`.
- Check out the new branch locally and set the upstream to the remote (push it once so both local and remote branches exist).
- If you can’t create a local branch (detached HEAD, missing permissions, or no remote access), stop and fix before starting work.
- Keep branches small and focused on one Issue.

## Coding Flow
1. Load issue/ticket.
2. Check if the ticket is self-explanatory. If not, ask clarification questions and update the ticket (plan + acceptance criteria).
3. Run `git fetch origin`, then create a local branch from `main`/`master` (whichever exists). In the same step, set the Issue's label to `status:in-progress` — see Status tracking, above.
4. Work in small units; keep working notes, decisions, TODOs, and risks.
   Record implementation history in the Issue/PR unless it establishes a durable architectural rule that future work must preserve.
5. Run diagnostics.
6. Document progress in the ticket (what’s done, what’s next).
7. Ask to continue if needed due to token limits.
8. If continuing and not done, go back to step 4.
9. Check Definition of Done (diagnostics, required tests/linters).
10. Ask to commit, push, and open a PR.
    - If the PR fully satisfies the Issue acceptance criteria, use `Fixes #<id>`.
    - If the PR is partial, use `Part of #<id>` and list remaining scope explicitly.

## Commits
Format: `type(scope): summary (#<id>)`
Examples:
- `feat(api): add order endpoint (#123)`
- `fix(core): handle null price (#123)`
- `docs(readme): clarify setup steps (#123)`

## Pull requests
- Title includes `(#<id>)`.
- Issue linkage in body must match scope:
  - Full-scope PR (all acceptance criteria done): `Fixes #<id>`.
  - Partial PR (some criteria still open): `Part of #<id>` (never `Fixes #<id>`).
- Include: Context, Approach, Trade-offs/Risks, How to test.
- Keep PRs reviewable; split if needed.

## Review & merge
- Request review when ready; update status to `status:review`.
- Address feedback; keep commits clean.
- **A security-relevant PR merges only after an independent review.** Security-relevant: cryptography, key material, certificates and identities, authentication and authorization, and anything that releases a secret. The reviewer is someone other than the author, given the code and the design, not the author's conclusions. Its findings are fixed and the fixes checked again before the merge. Other PRs keep the author's own review.
- Merge using the repo’s preferred strategy.

## Completion
- Ensure Definition of Done is satisfied.
- Update Issue plan/checklist.
- Set status to `status:done` and close the Issue.
- Link the PR in the Issue.

## Escalation
- If blocked or confidence is low, add `status:blocked` and comment with:
  - blocker
  - what’s needed
  - next action
