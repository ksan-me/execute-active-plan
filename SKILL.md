---
name: "execute-active-plan"
description: "Execute the active implementation plan for a local repository by reading AGENTS.md and the existing working docs, selecting unchecked work from docs/plan.md, implementing it, updating progress docs, and continuing until a real stop condition is reached. Use when the repository already has an execution layer and the user wants Codex to keep implementing instead of re-planning or rebuilding the package from raw ideas."
---

# Execute Active Plan

Execute the implementation layer of a repository that already has operating docs.

This skill is the execution half of a two-layer system:
- `brainstorm-to-build-package` designs and refreshes the execution package
- `execute-active-plan` reads that package and keeps implementation moving

Default behavior is continuation. Unfinished work is not a reason to stop.

Optimize for:
- stable continuation
- narrow active scope
- resumable progress
- low drift between code and docs
- explicit stop conditions
- safe lane-based parallel execution when defined

## Role Boundary

Treat this skill as the implementation and progress layer, not the planning layer.

Use this skill to:
- execute unchecked work already represented in repository docs
- keep code and execution docs aligned while implementing
- continue through multiple small batches without asking after every checkbox
- stay inside the active scope, milestone, release target, and lane boundaries

Do not use this skill to:
- turn raw brainstorm notes into a project package
- rewrite the repository strategy from scratch when the execution layer is missing
- promote broad backlog or future ideas into active work without documenting that change

If the repository does not have a usable execution layer, use `brainstorm-to-build-package` first.

## Inputs

Gather and use only repository reality and user-provided instructions.

Primary execution docs:
- `AGENTS.md`
- `docs/parallel-plan.md` if it exists
- `docs/plan.md`
- `docs/current-scope.md`
- `docs/release-target.md` if it exists
- `docs/mvp.md`
- `docs/milestones.md`

Execution state docs:
- `docs/progress.md` if it exists
- `docs/todo.md` if it exists

Supporting execution docs:
- `docs/execution-protocol.md` if it exists
- `docs/handoff.md` if it exists
- `docs/current-state.md` if it exists
- `docs/risk-register.md` if it exists
- `docs/verification-matrix.md` if it exists
- `docs/vision.md` if it exists

Archive context:
- `docs/idea-log.md` only when working docs are missing, contradictory, ambiguous, or the user explicitly asks to revisit the original intent

If required information is missing, infer the safest execution step from repository reality and repair the docs. Use `TODO` only when the repository and user context still do not answer the question safely.

## Required Context Order

Prefer this order unless a repository-specific document states a stricter order:

1. `AGENTS.md`
2. `docs/parallel-plan.md` if it exists
3. `docs/plan.md`
4. `docs/current-scope.md`
5. `docs/release-target.md` if it exists
6. `docs/mvp.md`
7. `docs/milestones.md`
8. `docs/execution-protocol.md` if it exists
9. `docs/handoff.md` if it exists
10. `docs/current-state.md` if it exists
11. `docs/risk-register.md` if it exists
12. `docs/verification-matrix.md` if it exists
13. `docs/todo.md` if it exists
14. `docs/progress.md` if it exists
15. `docs/idea-log.md` only if needed

Interpret that order this way:
- `AGENTS.md` defines repository-wide execution rules
- `docs/parallel-plan.md` defines lane ownership and overlap constraints
- `docs/plan.md` defines the immediate work queue
- `docs/current-scope.md` defines what is allowed now
- `docs/release-target.md` defines why execution must continue
- `docs/mvp.md` and `docs/milestones.md` explain priority and dependency order
- `docs/idea-log.md` is a recovery source, not the default driver

## Workflow

1. Read `AGENTS.md`.
2. Read `docs/parallel-plan.md` if it exists and determine the current lane, owned surfaces, risky overlap, dependencies, and allowed integration points before selecting work.
3. Read `docs/plan.md` and locate the first unchecked item that fits the current lane.
4. Read `docs/current-scope.md` and confirm the selected work is in active scope.
5. Read `docs/release-target.md` if it exists and treat remaining keep-going conditions or incomplete release checklist items as reasons to continue.
6. Read `docs/mvp.md` and `docs/milestones.md` to confirm dependency order and avoid pulling later work forward.
7. Read `docs/execution-protocol.md`, `docs/handoff.md`, `docs/current-state.md`, `docs/risk-register.md`, and `docs/verification-matrix.md` when they exist and would improve execution accuracy.
8. If `docs/plan.md` is missing, stale, over-broad, or exhausted while active release work remains, repair or refill it from `docs/current-scope.md`, `docs/milestones.md`, `docs/release-target.md`, and the active sections of `docs/todo.md`.
9. If the selected work is too broad to implement and verify safely in one pass, split it into thinner tasks in `docs/plan.md` before coding.
10. Inspect the repository and choose the smallest useful implementation batch:
    - one unchecked task
    - or a very small cluster of directly dependent unchecked tasks
11. Implement the required code and doc changes.
12. Run the most relevant checks:
    - targeted checks first
    - broader checks when useful
    - if a required command cannot be inferred safely, record `TODO` and continue with other unblocked work when possible
13. Update execution docs to match reality:
    - `docs/plan.md`
    - `docs/progress.md`
    - `docs/todo.md`
    - `docs/parallel-plan.md` when lane coordination changed
    - `docs/handoff.md` when a fast resume note exists
    - `docs/current-state.md`, `docs/risk-register.md`, or `docs/verification-matrix.md` when implementation changed their truth
14. Decide whether to continue:
    - continue if unchecked active work remains
    - continue if any release-target keep-going condition remains true
    - continue after doc updates without asking the user for confirmation
    - stop only on a real blocker or a truly satisfied release target
15. Repeat immediately with the next unchecked item until a real stop condition is reached.

## Execution Loop

Model the skill as a continuation loop:

1. Select the next execution target from `docs/plan.md`
2. Validate it against scope and lane boundaries
3. Implement it
4. Verify it
5. Record reality in the execution docs
6. Re-evaluate continuation conditions
7. Continue with the next unchecked item

Do not treat this as a single-task skill. Treat it as a loop that keeps moving until a real stop condition is reached.

## Execution Rules

- Execute from `docs/plan.md`, not from raw idea files and not from the full backlog.
- Treat unfinished work as a continuation condition, not a stop condition.
- Keep work inside `docs/current-scope.md`.
- If `docs/release-target.md` exists, keep going until it is satisfied or a hard blocker is reached.
- If `docs/parallel-plan.md` exists, stay inside the assigned lane unless explicit integration work is documented.
- Prefer repairing stale execution docs over ignoring them.
- Prefer repository reality over stale docs when they conflict, then update the docs.
- Prefer narrow, verifiable batches over broad partially-finished changes.
- Do not silently promote future ideas, stretch goals, or backlog themes into active execution.
- Do not stop after one completed checkbox if more in-scope active work remains.
- Do not ask the user for confirmation between normal plan steps when scope and stop conditions are already defined.
- Prefer fixing high-severity release-blocking risks before feature expansion when both compete for the same release target.

## Rules for `docs/plan.md`

Treat `docs/plan.md` as the live execution checklist.

Keep it:
- short
- concrete
- checkbox-driven
- implementation-oriented
- easy to resume after interruption

Keep this structure:
- `## Current objective`
- `## Current milestone`
- `## Checklist`
- `## Blockers`
- `## Assumptions`
- `## Next immediate action`
- `## Resume here`

Maintain it this way:
- mark completed items with `[x]`
- keep incomplete items as `[ ]`
- keep only 3 to 7 meaningful unchecked items when possible
- rewrite oversized items into thinner executable steps
- keep `Resume here` pointed at the first unchecked item or exact next action
- never leave it fully complete while release-target keep-going conditions still remain true

## Rules for `docs/parallel-plan.md`

Treat `docs/parallel-plan.md` as the lane boundary contract.

When it exists, expect it to define:
- lane names
- lane goals
- owned files or owned surfaces
- forbidden or risky overlap
- dependencies between lanes
- integration checkpoints
- current status
- lane-specific handoff notes

When updating it:
- keep boundaries explicit
- record coordination needs instead of crossing lanes silently
- add explicit integration tasks instead of informal overlap
- keep it concise enough that another Codex can join the lane quickly

## Rules for `docs/release-target.md`

Treat `docs/release-target.md` as the top-level continuation contract.

It should define when applicable:
- release target name
- `Must keep going while any of these remain true`
- release checklist
- hard stop conditions

Interpret it this way:
- if any keep-going condition remains true, continue
- if any release checklist item remains incomplete, continue
- do not reinterpret unfinished work as a valid reason to stop
- stop only when all required checklist items are complete and no keep-going condition remains true, or when a documented hard blocker applies

## Rules for `docs/progress.md`

Treat `docs/progress.md` as the durable execution log.

Record:
- current status
- milestone progress
- completed work
- changed files when useful
- notable decisions
- blockers that affect continuation

Do not turn it into a backlog or a roadmap.

## Rules for `docs/todo.md`

Treat `docs/todo.md` as the comprehensive backlog and overflow buffer.

Use it to capture:
- follow-up work discovered during implementation
- deferred scope
- blocked work
- later work that should not enter the active plan yet

Do not use it as the primary execution driver while `docs/plan.md` is usable.

## Rules for Optional Execution Docs

- `docs/execution-protocol.md`: treat as the canonical repository-specific run loop when it exists
- `docs/handoff.md`: keep it short and explicit so another Codex can resume in minutes
- `docs/current-state.md`: keep it factual and evidence-based
- `docs/risk-register.md`: keep active risks and findings visible, especially release-blocking ones
- `docs/verification-matrix.md`: keep proof status current so later sessions do not re-investigate blindly

## Stop Conditions

Stop only when one of these is true:

1. `docs/release-target.md` exists and all required release criteria are satisfied with no remaining keep-going conditions
2. a real blocker prevents safe continued execution
3. required permissions, dependencies, or commands are missing and cannot be inferred or obtained safely
4. the execution docs are contradictory enough that execution cannot continue safely without rebuilding the package
5. the assigned lane is exhausted or blocked and needs a new allocation or integration plan

Do not stop because:
- one checkbox was completed
- more work remains
- the next task is obvious
- the backlog is large

## Quality Check

Before finishing, verify that:

- work was driven by `docs/plan.md`
- active work stayed inside `docs/current-scope.md`
- if `docs/parallel-plan.md` exists, execution stayed inside the assigned lane or explicit integration tasks
- completed work is reflected in code or docs and in `docs/progress.md` when that file exists
- newly discovered follow-ups were moved to `docs/todo.md` instead of silently expanding scope
- `docs/plan.md` is still short and resumable
- oversized remaining items were split before leaving them active
- the repo can be resumed by another Codex from `AGENTS.md`, `docs/parallel-plan.md`, and `docs/plan.md`
- the skill did not stop merely because one task finished
- the skill did not confuse unfinished work with a satisfied stop condition
