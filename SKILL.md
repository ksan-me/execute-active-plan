---
name: "execute-active-plan"
description: "Execute the active implementation plan for a local repository by reading AGENTS.md and the working docs, following docs/plan.md step by step, updating progress as work completes, and continuing until a real stop condition is reached. Use when the repository already has planning docs and the user wants Codex to keep implementing instead of re-planning."
---

# Execute Active Plan

Execute the current implementation plan for an active repository.

This skill assumes the repository already has a maintained execution layer and should continue from that layer instead of re-planning from scratch. Default behavior is continuation, not early wrap-up.

Optimize for:
- stable continuation
- narrow scope execution
- frequent progress updates
- safe recovery after interruptions or compaction
- minimal drift between code and docs
- continuation toward a release target instead of premature summaries

## Inputs

Gather and use only what already exists in the repository or what the user explicitly provided:

- `AGENTS.md`
- `docs/execution-protocol.md` if it exists
- `docs/parallel-plan.md` if it exists
- `docs/handoff.md` if it exists
- `docs/current-state.md` if it exists
- `docs/risk-register.md` if it exists
- `docs/verification-matrix.md` if it exists
- `docs/plan.md`
- `docs/current-scope.md`
- `docs/release-target.md` if it exists
- `docs/mvp.md`
- `docs/milestones.md`

Secondary context:

- `docs/vision.md`
- `docs/todo.md`
- `docs/progress.md`

Archive context:

- `docs/idea-log.md` only when working docs are missing, ambiguous, contradictory, or the user explicitly asks to revisit original ideas

If required information is missing, repair the execution docs from repository reality where possible. Use `TODO` only when the repo and user context still do not answer the question safely.

## Purpose

Use this skill when the user wants Codex to:
- continue implementation from the current plan
- execute unchecked plan items without stopping after each small batch
- keep working toward a release target or milestone
- operate inside an assigned parallel lane when parallel work is defined
- update repo docs while implementing
- leave the repo in a resumable state for the next Codex

Do not use this skill to create a fresh project package from raw ideas. Use `brainstorm-to-build-package` first when the execution layer is missing.

## Required Context Order

Always prefer this order:

1. `AGENTS.md`
2. `docs/execution-protocol.md` if it exists
3. `docs/parallel-plan.md` if it exists
4. `docs/handoff.md` if it exists
5. `docs/release-target.md` if it exists
6. `docs/current-state.md` if it exists
7. `docs/risk-register.md` if it exists
8. `docs/verification-matrix.md` if it exists
9. `docs/plan.md`
10. `docs/current-scope.md`
11. `docs/mvp.md`
12. `docs/milestones.md`
13. `docs/todo.md`
14. `docs/progress.md`

## Workflow

1. Read `AGENTS.md` first.
2. Read `docs/execution-protocol.md` if it exists and follow it as the canonical run loop unless it clearly conflicts with repository reality.
3. If `docs/parallel-plan.md` exists, determine the current lane, owned surfaces, forbidden overlap, dependencies, and integration checkpoints before selecting work.
4. Read `docs/handoff.md` if it exists to capture the exact resume point, recent changes, and expected next move before scanning the checklist.
5. Read `docs/release-target.md` if it exists and treat its keep-going conditions as higher priority than any urge to summarize and stop.
6. Read `docs/current-state.md`, `docs/risk-register.md`, and `docs/verification-matrix.md` if they exist so execution is grounded in implemented reality, known risk, and actual verification coverage.
7. Read `docs/plan.md` and identify the first unchecked item that is inside the current lane and `docs/current-scope.md`.
8. If lane assignment is unclear, infer the safest lane from file ownership and update `docs/parallel-plan.md` to make the assignment explicit before proceeding.
9. If the active checklist is exhausted but release criteria are not satisfied, refill `docs/plan.md` from `docs/current-scope.md`, `docs/milestones.md`, `docs/risk-register.md`, and the `Active` or `Next` sections of `docs/todo.md` instead of stopping.
10. Inspect the repository and identify the smallest useful batch of work:
   - one unchecked task
   - or a very small cluster of directly dependent unchecked tasks
11. If the selected task is too broad to complete and verify in one run, split it into thinner executable subtasks in `docs/plan.md` before implementing.
12. Prefer discovering commands from repository manifests, scripts, CI config, package files, and `docs/verification-matrix.md` before concluding that commands are unknown.
13. Prefer fixing exploitable or high-severity findings before feature expansion when those findings are inside the current release target.
14. Make the required code and documentation changes.
15. Run the most relevant checks:
   - targeted tests first
   - broader checks when useful
   - if a required command truly cannot be inferred, record `TODO` and continue with other unblocked work when possible
16. Update `docs/plan.md`:
   - mark completed items with `[x]`
   - keep remaining unchecked items actionable
   - keep only 3 to 7 meaningful unchecked items when possible
   - rewrite any oversized remaining item into thinner tasks before leaving it active
   - refresh `Next immediate action`
   - refresh `Resume here`
17. Update `docs/parallel-plan.md` when lane status, integration notes, or coordination needs changed.
18. Update `docs/current-state.md` when repository reality materially changed.
19. Update `docs/risk-register.md` when a finding was mitigated, narrowed, superseded, or newly discovered.
20. Update `docs/verification-matrix.md` when checks were added, coverage changed, or a major surface remains unverified.
21. Update `docs/progress.md` with milestone status, completed work, notable decisions, and changed files when useful.
22. Update `docs/handoff.md` with the exact current lane, current status, last completed action, next command or file, blockers, and resume point.
23. Update `docs/todo.md` with newly discovered follow-up work, deferred scope, risks, or blockers.
24. Update `AGENTS.md` only when execution rules, commands, lane policy, milestone order, or release criteria materially changed.
25. Decide whether to continue:
   - continue automatically if unchecked active work remains and no hard blocker exists
   - continue automatically if any release-target keep-going condition remains true
   - continue automatically after doc updates without asking the user for confirmation
   - stop only on a real blocker, missing permissions or dependencies that prevent safe progress, contradictory docs that cannot be repaired from repo reality, or a truly satisfied release target
26. Repeat the workflow immediately for the next unchecked item until a real stop condition is reached.
27. Finish only after leaving the repo stable and resumable.

## Execution Rules

- Execute from `docs/plan.md`, not from the full backlog.
- Prefer continuing through multiple small task batches in one run instead of stopping after one checkbox.
- Unfinished work is a reason to continue, not a reason to stop.
- If `docs/release-target.md` exists, keep going until it is satisfied or a hard blocker is reached.
- If `docs/execution-protocol.md` exists, treat it as the preferred operating loop for long-running sessions.
- If `docs/current-state.md` or `docs/risk-register.md` contradict the active checklist, repair the checklist instead of blindly following stale items.
- If `docs/parallel-plan.md` exists, stay inside the assigned lane unless the docs explicitly allow integration work.
- If `docs/handoff.md` exists, keep it fresh enough that another Codex can resume within minutes rather than re-reading the whole repository.
- Prefer isolated file ownership and explicit merge points over informal parallel overlap.
- Do not ask the user for confirmation between active plan steps when scope and stop conditions are already defined.
- Keep scope narrow and local to the current milestone.
- Do not silently promote broad backlog items into active execution.
- Do not leave an oversized task as a single checkbox if it hides multiple days of work or multiple verification surfaces.
- When missing work is discovered, add it to `docs/todo.md` or refill `docs/plan.md` explicitly instead of broadening scope implicitly.
- Use repository reality over stale docs when they conflict, then repair the docs.
- Do not invent secrets, services, infrastructure, or commands.
- Do not stop merely to announce the next available task.
- Prefer security remediation before new features when both compete for the same release target and the remediation closes an exploitable gap.

## Rules for `docs/plan.md`

Treat `docs/plan.md` as the live execution checklist.

It must remain:
- short
- concrete
- checkbox-driven
- easy to resume after interruption

Keep this structure:

- `## Current objective`
- `## Current milestone`
- `## Checklist`
- `## Blockers`
- `## Assumptions`
- `## Next immediate action`
- `## Resume here`

Plan maintenance rules:
- mark completed tasks with `[x]`
- keep incomplete tasks as `[ ]`
- keep tasks implementation-oriented
- if `docs/parallel-plan.md` exists, scope checklist items to the current lane or explicit integration tasks
- remove stale completed items only after the outcome is captured in `docs/progress.md`
- keep no more than 3 to 7 meaningful unchecked items when possible
- never leave the checklist fully complete while release-target keep-going conditions still remain true
- make sure `Resume here` points to the first unchecked item or exact next action

## Rules for `docs/parallel-plan.md`

Treat `docs/parallel-plan.md` as the coordination file for long-running parallel execution.

It should define when applicable:
- lane names
- lane goal for each lane
- owned files or surfaces
- forbidden overlap or risky overlap
- dependencies between lanes
- merge or integration checkpoints
- current status for each lane
- explicit handoff notes for the next Codex working that lane

When updating it:
- keep lane boundaries explicit
- record new coordination needs
- add integration work instead of silently crossing lane boundaries
- keep it concise enough that another Codex can join quickly

## Rules for `docs/execution-protocol.md`

Treat `docs/execution-protocol.md` as the canonical Codex runbook.

It should define:
- context read order
- task batch size
- refill rules for `docs/plan.md`
- how to update operating docs after each batch
- when to continue automatically
- real stop conditions

Keep it procedural, compact, and optimized for long terminal sessions rather than human project status reading.

## Rules for `docs/handoff.md`

Treat `docs/handoff.md` as the fast-resume note for the next Codex.

It should capture:
- current objective
- active lane
- last completed action
- next command, file, or verification step
- blockers or cautions
- resume point

Keep it short enough to scan in under a minute.

## Rules for `docs/current-state.md`

Treat `docs/current-state.md` as the factual repository snapshot.

It should separate:
- implemented
- partially implemented
- stubbed or placeholder integrations
- unverified claims
- known gaps

When updating it:
- prefer evidence from code, config, tests, and docs already in the repo
- avoid roadmap language
- demote uncertain claims to `Unverified` instead of overstating them

## Rules for `docs/risk-register.md`

Treat `docs/risk-register.md` as the active risk and findings ledger.

It should capture:
- severity
- affected surface
- exploitability or impact
- planned mitigation
- current status

When a risk is selected for active work:
- break it into executable remediation tasks in `docs/plan.md`
- update the risk status after implementation or verification

## Rules for `docs/release-target.md`

Treat `docs/release-target.md` as the highest-level continuation contract.

It should define when applicable:
- release target name
- `Must keep going while any of these remain true`
- release checklist
- hard stop conditions

Interpretation rules:
- if any keep-going condition remains true, continue execution
- if release checklist items remain incomplete, continue execution
- do not reinterpret unfinished work as a stop condition
- only treat the release target as satisfied when all required checklist items are complete and no keep-going condition remains true

## Rules for `docs/progress.md`

Treat `docs/progress.md` as the durable execution log.

Record:
- current status
- milestone progress
- completed tasks
- changed files when relevant
- notable decisions
- blockers if they affect execution

Do not turn `docs/progress.md` into a backlog or speculative roadmap.

## Rules for `docs/verification-matrix.md`

Treat `docs/verification-matrix.md` as the proof ledger.

It should map major surfaces to:
- verification command or method
- expected success signal
- last known status
- current gaps or TODO checks

Use it to reduce duplicate investigation when sessions span hours or hand off between agents.

## Rules for `docs/todo.md`

Treat `docs/todo.md` as the comprehensive backlog.

Group items into:
- Active
- Next
- Deferred
- Later
- Blocked

Use it as overflow backlog when `docs/plan.md` must stay short.

Do not bury high-severity security findings only in `docs/todo.md`; keep them visible in `docs/risk-register.md`.

## Stop Conditions

Stop only when one of these is true:

1. `docs/release-target.md` exists and all required release criteria are satisfied with no remaining keep-going conditions
2. a real blocker prevents safe continued execution
3. required permissions, dependencies, or commands are missing and cannot be inferred or obtained safely
4. the working docs are contradictory enough that replanning is required and cannot be repaired from repo reality
5. the assigned parallel lane is exhausted or blocked and needs reallocation or a new integration plan

Do not stop because one checkbox was completed.
Do not stop because more work remains.
Do not stop only to request confirmation for the next active plan step.

## Quality Check

Before finishing, verify that:

- work was driven by `docs/plan.md`
- active scope stayed aligned with `docs/current-scope.md`
- if `docs/parallel-plan.md` exists, execution stayed inside the assigned lane or explicit integration tasks
- any cross-lane coordination needs were recorded instead of handled implicitly
- no backlog item was silently promoted into active execution
- completed work is reflected in code or docs and in `docs/progress.md`
- `docs/handoff.md` is fresh enough that another Codex can restart quickly
- `docs/verification-matrix.md` reflects newly added checks or still-unverified surfaces
- `docs/plan.md` is still short and resumable
- oversized work items were split before being left as active tasks
- the skill did not stop merely because one checkbox was completed while active work still remained
- the skill did not mistake unfinished work for a satisfied stop condition
- if `docs/release-target.md` exists, stopping happened only because it was truly satisfied or a hard blocker was reached
- the repo can be resumed by another Codex reading `AGENTS.md`, `docs/parallel-plan.md`, and `docs/plan.md`
