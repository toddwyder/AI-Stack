# Issue 40 — gsd-core decision register

Status: agreed direction from the issue-40 review. This converts the raw functional census into migration decisions. It is not an implementation plan.

Companion evidence: `ISSUE_40_GSD_CORE_RAW_FUNCTIONAL_CENSUS.md`.

## 1. Governing principles

1. GitHub Issues and Projects are the authoritative workflow and backlog system.
2. Use GitHub comments unless a comment is the wrong container because the content requires durable repository discovery, structure, versioning, reuse, or substantial evidence.
3. Produce the fewest process artifacts possible. Do not recreate gsd-core under new names.
4. Durable records preserve practical engineering traceability, not every intermediate conversation or agent action.
5. Load only the current issue's relevant material. Never ingest all historical specs, documents, or traces by default.
6. Every retained capability is classified at two levels: does it belong in AI-Stack for all projects; if not, does this project specifically need it?
7. A missing necessary capability becomes a GitHub migration issue. The inventory itself does not expand into implementation.

## 2. Target issue workflow

### Authoritative state

- GitHub Project status replaces `STATE.md`.
- Initial statuses: `Specifying`, `Awaiting approval`, `Implementing`, `Verifying`, and `Done`.
- Blocked work uses a visible GitHub `Blocked` status or label plus a comment naming the blocker.
- Agents update GitHub status directly at meaningful transitions. Do not build a separate state engine initially.
- GitHub Issues and Projects fully replace `ROADMAP.md`.

### Specification and approval

- Every implementation issue has a permanent spec under `specs/`, named predictably by GitHub issue number and subject.
- Trivial specs may be only a few lines.
- Each spec contains minimal metadata: governing issue, approval status, and any superseded spec.
- The exact approved spec is the authoritative requirements record and remains discoverable after issue closure.
- When a requirement changes, create a new spec and mark the previous spec as superseded. Do not silently rewrite approved history.
- During implementation, a required scope change pauses work until a revised or superseding spec receives explicit approval.
- Agents load only the current spec and explicitly linked predecessor or related specs.

### Implementation proposal

- Initially, every issue requires Todd's explicit GitHub approval before implementation begins.
- The proposal normally lives in a GitHub comment and contains:
  - intended change;
  - key implementation approach;
  - verification plan, including tests/checks and evidence to present;
  - independent-review decision and rationale;
  - any applicable specialist reviews: security, UI, accessibility, AI evaluation, and observability;
  - TDD approach, or an explicit reason TDD will not be used.
- A separate `plan.md` is temporary and used only when a comment is an unsuitable container.
- Developers may work autonomously within an approved issue but may not advance into another issue without fresh approval.

### Review and verification

- Independent code review is selected using proposal-time judgment, not imposed on trivial work and not controlled by a new classification system.
- Review is normally warranted for business logic, security/authentication, persisted data, external integrations, migrations, concurrency, and interacting components.
- Independent AI review may be requested for complex proposals or implementations.
- Review findings, resolution, and reviewer identity are summarized on the issue. Full transcripts are retained only when necessary evidence.
- Appropriate automated testing is universal. TDD is the default; opting out requires an explicit reason in the approved proposal.
- Every acceptance criterion maps to a test, check, or piece of evidence.
- The acceptance-to-evidence mapping normally appears in the completion comment; use a separate verification document only when too large.
- Verification evidence normally attaches or links directly to the issue.
- Guided UAT remains standard for user-visible behavior. Internal changes use automated and targeted verification.
- Guided UAT checklists and pass/fail results remain in the closed issue; separate UAT documents are exceptional.

### Risks and completion

- A risk that blocks acceptance keeps the current issue open.
- An accepted risk requiring future action becomes a linked GitHub issue.
- A minor accepted limitation requiring no action may be stated in the completion comment.
- Retire `CONCERNS.md`, phase concern files, and roadmap concern tracking.
- A normal completion comment records what changed, meaningful deviations, test/check results, acceptance-to-evidence mapping, UAT when applicable, and linked follow-up risks.
- A separate `result.md` is optional and used only for evidence or reusable operational information too substantial for a concise comment.
- After the completion record is captured, delete temporary per-issue work folders, working plans, prototypes, and handoffs.
- Every implementation commit and pull request references its GitHub issue.

## 3. Durable repository documents

### Always or generally retained

- `PROJECT.md`: concise vision, durable constraints, and operating principles.
- `specs/`: permanent approved requirement records keyed by issue.
- `learning.md`: curated, subsystem/topic-organized knowledge that has no better authoritative home.

### Created when the project needs them

- `ARCHITECTURE.md`: intentional system boundaries, ownership, data flow, and cross-cutting rules that cannot be inferred safely from code.
- `DESIGN.md`: project-wide design principles, tokens, interaction patterns, and accessibility expectations.
- `AI.md`: for AI projects, project-wide provider/model strategy, safety and privacy boundaries, evaluation standards, observability, cost controls, prompt/versioning conventions, and fallback policy.

### Knowledge-routing rule

At issue completion, route durable knowledge to the most appropriate home:

1. `PROJECT.md` for vision, durable constraints, or operating principles.
2. `ARCHITECTURE.md` for intentional system boundaries and reusable technical contracts.
3. `DESIGN.md` for reusable product-design rules.
4. `AI.md` for reusable AI-system policy.
5. `learning.md` only for non-obvious facts, traps, API/tool behavior, or defect root causes without a more natural authoritative home.

A `learning.md` entry is limited to a short fact, consequence, and source-issue link. When work changes a subsystem, completion updates or removes obsolete entries for that subsystem. A learning is promoted to `ARCHITECTURE.md` when it becomes an intentional rule that multiple future changes must respect; move it rather than duplicate it.

### Julia-specific document migration

- Move `.planning/learning.md` to a curated root `learning.md`.
- Extract still-current architectural contracts from `.planning/codebase/` into a curated root `ARCHITECTURE.md`; do not retain the generated codebase-document suite.
- Retain and curate root `DESIGN.md`.
- Create `AI.md` only if Julia's durable AI-wide policy warrants it.
- Before deleting `.planning/`, extract:
  - current requirements into `specs/`;
  - reusable knowledge into the appropriate durable documents;
  - unresolved work into GitHub issues.
- Rely on Git history for legacy detail after extraction.

## 4. Transient and derived information

- Research is transient. Only decision-relevant conclusions and source links survive in the approved spec.
- Discussion remains in GitHub history. Do not create separate discussion logs or context documents; the approved spec holds durable conclusions and only necessary rationale.
- Pocock handoffs are temporary and stored outside the repository; delete after successful resumption or issue closure.
- Debug hypotheses, experiments, and findings normally use issue comments. Pocock diagnosing-bugs replaces GSD debug and forensics persistence.
- Execution traces are diagnostic snapshots. Retain only when explicitly attached as evidence for an incident or process experiment.
- Progress dashboards, milestone summaries, statistics, and snapshot reports are generated on demand and are never authoritative state.
- Graphify outputs are derived, rebuildable, gitignored, and kept in a tool-owned location outside `.planning/`.
- Technical and UI prototypes are throwaway. Preserve only the answered question and resulting decision in the issue or spec.

## 5. Retained capabilities

| Capability | Decision | Scope |
|---|---|---|
| GitHub issue/project workflow | Retain as authority | AI-Stack baseline |
| Pocock specification and setup flow | Retain | AI-Stack baseline |
| Pocock handoff | Retain as transient continuity | AI-Stack baseline |
| Pocock Ask Matt | Retain; replaces GSD user profiling | AI-Stack baseline |
| Pocock triage | Retain; replaces GSD inbox/backlog review | AI-Stack baseline |
| Pocock diagnosing-bugs | Retain; replaces GSD debugger/forensics | AI-Stack baseline |
| Pocock prototype | Retain for runnable logic and UI uncertainty | AI-Stack baseline |
| Bounded technical experiments | Retain as ordinary issue-scoped work when Pocock prototype is the wrong shape | AI-Stack baseline |
| Guided UAT | Retain for user-visible changes | AI-Stack baseline |
| Independent plan/code review | Retain when selected by risk/complexity | AI-Stack baseline |
| Specialist review | Retain conditionally: security, UI, accessibility, AI evaluation, observability | AI-Stack baseline |
| Acceptance-to-evidence mapping | Retain as closure requirement | AI-Stack baseline |
| Graphify | Retain independently as optional context-efficiency tooling | Project decision; enabled for Julia |
| Curated learning capture | Retain with strict admission and promotion rules | AI-Stack baseline |
| Native host agents/models | Retain; replaces GSD custom roster and profiles | Host capability |
| Native Git/GitHub branches, worktrees, PRs, and reverts | Retain; replaces GSD equivalents | AI-Stack baseline |

Every new project setup checklist explicitly records `Graphify: enable / not needed` with rationale so the decision is not forgotten.

## 6. Retired gsd-core capabilities and artifacts

- Remove the GSD command, skill, agent, installer, capability-management, and hook runtime as a whole after replacement validation.
- Retire `ROADMAP.md`, `STATE.md`, `REQUIREMENTS.md`, `.planning/config.json`, GSD phase/milestone directories, generated codebase maps, discussion/context artifacts, summaries, validation files, UAT files, review files, concern files, dashboards, active-workstream stores, and commit manifests.
- Retire separate GSD fast/quick modes. Trivial issues use the same workflow with shorter specs and proposals.
- Retire GSD capture, seeds, todos, inbox, and backlog files; GitHub Issues are the sole backlog and idea store.
- Retire GSD workspaces, workstreams, manager dashboards, model profiles, custom agent roster, statusline state, health/repair subsystem, safe-undo manifest, PR-branch filtering, milestone archive machinery, MemPalace integration, user profiling, sketch/spike commands, debugging persistence, and documentation-generation workflow.
- Do not port an uncovered capability preemptively. Review it, apply the AI-Stack/project-specific test, and create a migration issue only when necessary.

## 7. Runtime and repository instruction model

- Maintain one lean canonical shared instruction source.
- `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, and custom-GPT instructions contain only harness-specific bootstrap material and a pointer to the shared source.
- Keep repository identity, critical non-negotiable rules, and authoritative links in the shared entry point.
- Put procedural detail in skills and mechanical enforcement in hooks rather than expanding instruction files.
- Julia instruction surfaces must replace `.planning/` and `gsd/active/` references with GitHub, `specs/`, `PROJECT.md`, `ARCHITECTURE.md`, `DESIGN.md`, `AI.md` when present, and `learning.md`.

## 8. Required migration work

Create separate GitHub migration issues for at least:

1. Establish the lightweight GitHub issue template and minimal Project statuses.
2. Establish `specs/` naming, metadata, approval, and supersession conventions.
3. Create/curate shared canonical agent instructions and lean harness-specific entry points.
4. Extract Julia's current requirements, unresolved work, architecture, design/AI policy, and reusable learnings from `.planning/`.
5. Move Graphify to an independent minimal configuration and gitignored tool-owned output location.
6. Remove Julia `gsd-sync-check.js` and its pre-commit planning-file coupling.
7. Disable `.github/workflows/gsd-sync.yml` when the replacement process becomes active.
8. Retire Julia `gsd/active/` and update all repository references.
9. Reconcile and replace Julia imports of `recipeDbSchema` and `clipperInboxSchema` from `gsd-core` with Julia-owned schemas; test affected validation, Firebase write, and external-data-gateway paths.
10. Review each Julia bouncer/guardrail twice: AI-Stack baseline applicability, then Julia-specific necessity. Classify each as AI-Stack, Julia-only, or remove.
11. Remove gsd-core package/runtime dependencies after all required validation and decoupling work passes.
12. Delete the legacy `.planning/` tree only after extraction and verification of retained information.

## 9. Must validate before gsd-core removal

| Validation gate | Reason | Minimum acceptable outcome |
|---|---|---|
| Context-limit awareness | Agents do not reliably know exact remaining context capacity | Pocock or host provides a reliable warning-to-handoff path; otherwise retain only a minimal threshold warning |
| Untrusted-content protection | External documents, issues, and web content can contain prompt injection | Confirm host protection is adequate; if not, add the smallest missing safeguard rather than porting GSD hooks |
| Specification audit trail | Prevent requirement/hallucination ambiguity | Approved spec remains exact, discoverable, linked to issue, and supersession works |
| Workflow state | Replace `STATE.md` without ambiguity | GitHub status transitions and blocker handling work in practice |
| Proposal approval | Preserve Todd's initial review gate | No implementation begins without recorded approval |
| Verification completeness | Prevent skipped acceptance criteria | Every criterion has test/check/evidence mapping and UAT where applicable |
| Handoff continuity | Preserve interrupted work without persistent state machinery | Pocock handoff successfully resumes a real interrupted issue and is removed afterward |
| Graphify independence | Preserve proven context reduction | Julia can build/query Graphify without gsd-core or `.planning/` |
| Julia application decoupling | gsd-core currently exports live application schemas | No application import or runtime/package dependency remains; targeted tests pass |
| Instruction leanness | Avoid context bloat and cross-harness drift | Shared canonical source plus minimal harness-specific bootstraps work in each adopted harness |

## 10. Deferred process optimization

- Start with explicit approval on every issue. If cumbersome, streamline later using observed friction rather than designing exemptions now.
- Start with direct agent updates to GitHub status. Add automation only if manual transitions prove unreliable.
- Start with judgment-based review selection. Add rules or tooling only if decisions become inconsistent.
- Create snapshot reports only when someone needs them; never maintain them as parallel state.
- Review necessary capabilities before removal, but require demonstrated value before rebuilding any GSD mechanism.
