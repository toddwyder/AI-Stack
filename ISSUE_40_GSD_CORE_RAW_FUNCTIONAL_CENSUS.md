# Issue 40 — gsd-core raw functional census

Status: discovery inventory only. No disposition decisions are made here.

Evidence snapshot: `toddwyder/AI-Stack` and `toddwyder/Julia`, default branches, cloned 2026-08-17. The package inspected is the vendored `Julia/private-packages/gsd-core` package (`package.json` version 1.6.0) plus Julia-owned integrations and live artifacts.

## 1. Surface and packaging

The vendored package is a meta-prompting, context-engineering, and spec-driven development system, not a single library. Its same underlying workflows are projected into several runtime surfaces:

- 69 command definitions and 69 skill packages expose user entry points.
- 89 workflow files contain the underlying process instructions; command and skill files are often adapters to these workflows, not separate functions.
- 34 specialized agent prompts implement research, planning, execution, verification, review, documentation, security, UI, evaluation, profiling, debugging, and synthesis roles.
- 46 artifact templates define persistent and transient records.
- 32 capability overlays provide optional functions and runtime-specific support.
- 20 top-level hook files provide event-driven guards, context injection, status, and maintenance.
- The installer projects commands, skills, agents, hooks, configuration, and runtime binaries into Claude, Codex, Gemini, Copilot, Cursor, Windsurf, Trae, Cline, Augment, Qwen, CodeBuddy, Hermes, Antigravity, OpenCode, and Kilo layouts.
- `gsd-tools` is the machine-facing query/mutation/validation CLI; `gsd_run` locates and launches it across runtime layouts.

Lifecycle: package content is upstream-derived and vendored into Julia; installed runtime copies are generated/managed artifacts. Project records under `.planning/` are user/project-owned and outlive an individual invocation.

## 2. Skills and process capabilities

The rows below group aliases, namespace routers, command wrappers, agents, and workflow fragments by the job they perform.

| Functional group | Function | Producer / mutator | Consumer | Lifecycle and dependencies |
|---|---|---|---|---|
| Project initialization | Elicit project intent, research ecosystem, scope requirements, establish roadmap and initial state | `new-project`; project researchers, synthesizer, roadmapper | All downstream phase workflows and humans | One-time/bootstrap; produces PROJECT, REQUIREMENTS, ROADMAP, STATE, config, research set; depends on user decisions, web research, existing codebase map |
| Existing-doc ingestion | Classify ADR/PRD/SPEC/docs, synthesize them, detect locked conflicts, bootstrap or merge planning state | `ingest-docs`; classifier and synthesizer agents | Project initialization/planning and humans resolving conflicts | Bootstrap or occasional merge; consumes repository docs; produces classified context and conflict records |
| Brownfield mapping and intel | Parallel codebase analysis; record architecture, structure, stack, conventions, tests, integrations, concerns; maintain queryable intel | `map-codebase`, intel updater, codebase mappers | Discuss, research, planning, review, future sessions | Persistent but refreshable `.planning/codebase/` and `.planning/intel/`; depends on source tree and repository history |
| Specification | Clarify what a phase must deliver, score ambiguity, probe edge completeness and prohibitions | `spec-phase` plus spec probes | Discuss/research/plan and human approval | Per phase, before context/planning; produces SPEC and probe findings |
| Phase discussion | Find gray areas, gather decisions, support default/auto/batch/chain/advisor/power/assumptions modes, retain audit trail | `discuss-phase`, advisor researcher, assumptions analyzer | Phase researcher, planner, UI/AI design flows, humans | Per phase; produces CONTEXT, DISCUSSION-LOG, optional checkpoint state; consumes roadmap, prior context, codebase map and user profile |
| UI design contract | Detect design-system state and lock visual/interaction standards | `ui-phase`, UI researcher/checker | Planner, executor, UI reviewer | Per frontend phase; produces UI-SPEC; consumes context, code and design assets |
| AI system design contract | Choose framework, document AI architecture/evaluation/guardrails/monitoring | `ai-integration-phase`; framework selector, AI/domain researchers, eval planner | Planner, executor, eval reviewer | Per AI phase; produces AI-SPEC; depends on official framework research and domain context |
| Phase research | Research implementation approach, version-specific patterns, risks and dependencies | phase researcher and project-specific attached skills | Planner and plan checker | Per phase, refreshable; produces RESEARCH; consumes CONTEXT/SPEC, codebase docs and configured research gates |
| Phase planning | Create executable PLANs, dependencies/waves, verification criteria, reachability checks; supports MVP slicing and cloud ultraplan | `plan-phase`, `mvp-phase`, `ultraplan-phase`, planner, pattern mapper, plan checker | Executor, verifier, humans | Per phase; plans are durable work records; depends on roadmap, requirements, context/research, git/code state |
| Cross-AI plan convergence | Send plans to external AI reviewers and replan until concerns converge | `review`, `plan-review-convergence` | Planner/human approval | Per plan or phase; depends on configured external AI CLIs/scripts and timeout/review settings |
| Execution | Execute plan tasks, order work in waves, spawn workers, use atomic commits, checkpoints, TDD mode and worktree safety | `execute-phase`, executor; execute-plan workflow | Verification, progress/state, git history | Per plan/phase; consumes PLAN and config; mutates source, SUMMARY, STATE, manifest/commit history; depends on tests, git, optional worktrees/cross-AI execution |
| Autonomous orchestration | Run discuss → plan → execute across remaining phases with stop/escalation gates | `autonomous` and auto-advance/next routing | Human at configured gates; normal phase consumers | Milestone-length controller; depends on every phase workflow, failure classification and config gates |
| Fast/quick task execution | `fast` performs trivial inline work; `quick` keeps atomic commits/state with reduced agent overhead | `fast`, `quick` | Human, git, quick-task history | Per small task; quick produces records under `.planning/quick/`; fast is intentionally lighter/transient |
| Freeform situational routing | Infer current position and route freeform intent to next valid workflow; show progress | `progress`, `next`, namespace routing skills | User and all downstream workflows | Session/read-side function; consumes ROADMAP, STATE, phase artifacts, gates and safety predicates |
| Phase/milestone CRUD | Add/insert/edit/remove phases, start/complete milestones, archive old phase material, plan milestone gaps | `phase`, `new-milestone`, `complete-milestone`, `cleanup`, transition/graduation workflows | Roadmap/state readers and project history | Durable project lifecycle; mutates ROADMAP, PROJECT, REQUIREMENTS, STATE, milestones/archive directories and git tags depending on config |
| Milestone audit and summary | Verify completion against original intent and requirements; synthesize onboarding/review summary | `audit-milestone`, `milestone-summary`, integration checker | Human release decision and next milestone | End-of-milestone; produces audit and summary artifacts; consumes phase verification, requirements and git state |
| Conversational UAT | Walk a human through behavioral verification and track pass/fail/deferred outcomes | `verify-work` | Audit UAT, verifier, human sign-off | Per phase/feature; produces/updates UAT and verification records; depends on a runnable product and user observation |
| Nyquist validation | Audit requirement-to-test coverage and fill validation gaps | `validate-phase`, nyquist auditor | Phase verification and milestone audit | Per completed phase; produces VALIDATION and tests; consumes requirements, plans, implementation and test suite |
| Goal verification | Goal-backward check that implementation achieves phase intent, not merely task completion | verifier and verify-phase workflow | Progress, milestone audit, ship | Per phase; produces VERIFICATION; consumes REQUIREMENTS, ROADMAP, PLAN, SUMMARY, tests and code |
| Code review and repair | Review changed source for correctness/security/quality and optionally apply atomic fixes | `code-review`, reviewer and fixer agents | Ship/merge and human | Per phase or diff; produces REVIEW and commits; depends on git diff, source and tests; optional Fallow pre-pass |
| Security design/enforcement | Threat-model plans, enforce security gates and retroactively verify mitigations/ASVS level | security capability, `secure-phase`, security auditor, prompt scanners/guards | Planner, executor, ship | Per plan/phase plus continuous hook/CI gates; produces SECURITY/review findings; depends on config severity and scanners |
| UI audit | Six-pillar visual review with optional Playwright-MCP evidence and human verification | `ui-review`, UI auditor | Human and remediation planning | Post-implementation frontend phase; produces UI-REVIEW/visual evidence; depends on runnable UI and browser tooling |
| AI evaluation audit | Compare implemented AI system to AI-SPEC evaluation strategy and score coverage | `eval-review`, eval auditor | Human and remediation plan | Post-implementation AI phase; produces EVAL-REVIEW; consumes AI-SPEC, implementation and evals |
| Test generation | Generate tests for completed behavior from UAT and implementation | `add-tests` | Test runner, verifier | Post-implementation; mutates test suite; consumes UAT, plan/summary and source |
| Debugging | Scientific-method diagnosis with persistent hypothesis/evidence state, context-reset recovery, diagnostic-only mode | `debug`, debugger and debug-session manager | Human, later debug sessions, fix workflow | Incident-length; produces DEBUG state under `.planning/debug/`; depends on reproduction, logs/tests and checkpoints |
| Forensics | Post-mortem failed GSD workflows and identify process failure causes | `forensics` | Process maintainer/human | After workflow failure; durable report; consumes artifacts, logs, state and git history |
| Audit-to-fix | Scan, classify, repair, test and commit issues autonomously | `audit-fix` | Human and git | Bounded repair run; depends on audit tooling, test suite and safety gates |
| Health and node repair | Diagnose planning-directory consistency, stale/missing nodes and optionally repair | `health`, node-repair | All artifact-driven workflows | On demand and recovery; reads/mutates planning graph/state; depends on expected artifact schema |
| Pause/resume and handoff | Capture exact continuation state and restore it across sessions/context loss | `pause-work`, `resume-work`, session-state hooks | Next agent/session and human | Mid-work transient-to-durable handoff; produces CONTINUE/HANDOFF-style records and reads STATE/active plan/git |
| Persistent context threads | Maintain named cross-session threads independent of a single phase | `thread` | Later sessions/workflows | Persistent project memory; stored in planning area; depends on thread store/CLI |
| Capture, seeds, backlog | Route ideas/tasks/notes/seeds; review and promote parked backlog work | `capture`, `review-backlog`, note/seed/backlog workflows | Future planning and humans | Persistent inbox/backlog until promoted/closed; depends on planning store and, for inbox, GitHub issues/PRs |
| GitHub inbox triage | Review open issues and PRs against repository templates/contribution rules | `inbox` | Maintainer/human | On demand; external live state; depends on GitHub access, issue/PR metadata and repository guidance |
| Workspaces/workstreams | Create isolated work environments; manage parallel named streams and aggregate status | `workspace`, `workstreams`, manager dashboard | Parallel agents/developers and integration | Work-duration lifecycle; depends on git worktrees/branches, base-ref checks and active-workstream store |
| Safe undo | Revert phase/plan commits using manifest and dependency checks | `undo` | Human and downstream state | Exceptional recovery; depends on git and phase commit manifest; mutates history via safe revert commits |
| PR preparation and shipping | Filter planning-only commits into clean PR branch; run review, create PR and prepare merge | `pr-branch`, `ship` | GitHub reviewers/human | End-of-work; depends on git/GitHub CLI, verification predicates, review configuration and optional PR-body custom sections |
| Documentation generation | Create/update project docs and verify factual claims against live code | `docs-update`, doc writer/verifier | Team and future agents | Persistent, refreshable; depends on codebase evidence and selected doc types |
| Developer profiling | Analyze session behavior and publish a discoverable user/developer profile | `profile-user`, user profiler | Discussion/planning agents and runtime instruction injection | Cross-project/user-level persistent profile; depends on session exports; affects language and workflow adaptation |
| Learning extraction | Extract decisions, lessons, patterns and surprises from completed phase records | `extract-learnings` | Future research/planning, humans, MemPalace | End-of-phase; produces phase LEARNINGS from PLAN, SUMMARY, VERIFICATION, UAT and STATE |
| MemPalace memory | Capture artifacts and temporal decision facts; recall cross-session decisions/patterns/surprises; curate at ship | `mempalace-capture`, `mempalace-recall`, curator agent | Planners, researchers, future sessions | Cross-session/cross-project persistent external memory; depends on MemPalace capability/tooling and source artifacts |
| Knowledge graph / Graphify | Build/query/status/diff a structural knowledge graph, freshness against commits, reports and HTML | `graphify`, CLI adapter and graphify update hooks | Agents/humans exploring architecture and change impact | Refreshable derived cache under `.planning/graphs/`; depends on external `graphify` Python CLI, git, config enablement and graph artifacts |
| Exploration, sketch and spike | Socratic ideation; throwaway UI mockups; experiential technical probes; route results onward | `explore`, `sketch`, `spike` | User and later specification/planning | Pre-commitment/experimental; outputs may be transient or wrapped into retained discovery records |
| Import | Safely ingest an external plan with conflict detection before writes | `import` | Planner/executor | Occasional; produces converted plan/conflict output; depends on project decisions and untrusted-input controls |
| Configuration and surface management | Manage workflow/model/safety/integration settings; expose/hide skill clusters without reinstall | `config`, `settings`, `surface` | Installer, router, every workflow/hook | Persistent per-project and global config; depends on schema/default manifests and runtime artifact layout |
| Statistics/manager dashboards | Summarize phases, plans, requirements, git metrics, timelines, workstreams | `stats`, `manager` | Human operator | Read-side/on demand; derived from planning artifacts and git |
| Update/install/migration | Install/update/uninstall runtime artifacts, detect versions, preserve user artifacts, migrate legacy layouts and GSD2 data | installer, `update`, sync launcher, migrations | Runtime command/skill consumers | Package/runtime lifecycle; depends on Node/npm, runtime homes, manifests, network and filesystem permissions |

Namespace skills (`ns-workflow`, `ns-review`, `ns-project`, `ns-context`, `ns-ideate`, `ns-manage`) are two-stage routers over the functions above. `help` is a discoverability layer. `surface` controls which of these functions appears in a runtime without changing the underlying installation.

## 3. Artifact, template, and state census

| Artifact family | Function | Producer | Consumer | Lifecycle / authority / dependencies |
|---|---|---|---|---|
| `PROJECT.md` | Vision, constraints, decisions and project evolution rules | new-project/new-milestone/ingest | roadmap, discussion, research, planning, audits, humans | Project-long durable planning authority inside GSD |
| `REQUIREMENTS.md` | Scoped, uniquely identified requirements and traceability | new-project, milestone requirements, ingestion | roadmap, plans, verification and audits | Milestone/project durable; changed under approval gates |
| `ROADMAP.md`, archive, milestone records | Phase sequence/status, requirement mapping and backlog | roadmapper, phase/milestone CRUD, progress | virtually every workflow | Project-long operational authority for GSD; archival copies preserve history |
| `STATE.md` | Current position, blockers, decisions, metrics and continuation state | initialization, progress, execute, transition, pause/resume | session startup/statusline, agents, humans | Frequently mutated project state; injected into sessions by installer/hooks |
| `config.json` | Workflow, gates, models, parallelism, safety, hooks, Graphify and integrations | initialization/config/settings | installer, router, workflows, hooks and CLI | Persistent configuration authority; per-project values override global defaults |
| `{phase}-SPEC.md` | Clarified deliverable, ambiguity and edge/prohibition constraints | spec-phase | discussion, research, planning | Per-phase durable design input |
| `{phase}-CONTEXT.md`, `DISCUSSION-LOG.md`, checkpoint JSON | User decisions, rationale/audit trail, resumable discussion state | discuss-phase | researcher/planner/human | Context and log durable; checkpoint is temporary workflow state |
| `{phase}-UI-SPEC.md` | UI design contract | UI phase | planner/executor/UI audit | Per-frontend-phase durable contract |
| `{phase}-AI-SPEC.md` | AI architecture, evaluation, guardrails and monitoring contract | AI integration phase | planner/executor/eval audit | Per-AI-phase durable contract |
| `{phase}-RESEARCH.md`; project research set | Implementation/domain evidence, stack, patterns, pitfalls and synthesis | research agents | planners/humans | Refreshable evidence; project research bootstrap vs per-phase research |
| `{phase}-{plan}-PLAN.md`, prompt templates | Executable tasks, dependencies, verification criteria and wave metadata | planner/import/MVP planner | executor, verifier, undo/manifest | Durable work specification; prompt templates are package scaffolding |
| `{phase}-{plan}-SUMMARY.md` variants | What changed, decisions, commits, deviations and handoff facts | executor | verification, learning extraction, milestone audit | Durable per-plan completion history |
| `VERIFICATION.md`, verification report | Goal/requirement evidence, gaps and status | verifier, Julia custom process | progress, audit, ship, human | Per-phase or root durable quality record |
| `UAT.md` | Conversational test cases and observed outcomes | verify-work | audit-UAT, verifier, human | Per feature/phase until resolved; human-observation record |
| `VALIDATION.md` | Nyquist requirement-to-test coverage and debt | validator/auditor | verifier/audit/test generation | Per phase, refreshable as tests change |
| `SECURITY.md` | Threats, mitigations, evidence and residual findings | planner/security auditor | executor, ship, human | Per phase durable security record |
| `UI-REVIEW.md`, `EVAL-REVIEW.md`, `REVIEW.md` | Specialized post-build audit findings and remediation | respective auditors/reviewer | fix workflows/human | Per review snapshot, retained for auditability |
| `LEARNINGS.md`, Julia `.planning/learning.md` | Decisions, lessons, patterns, surprises; Julia variant is constrained to 1–2 high-value bullets per phase | extract-learnings or Julia agents per GEMINI guidance | later agents/planning/humans/MemPalace | Cross-phase durable knowledge; depends on completed artifacts |
| `.planning/codebase/*.md` | Architecture/structure/stack/conventions/testing/integrations/concerns | codebase mappers/intel update | all code-aware phases and Julia guidance | Durable but refreshable derived documentation |
| Debug/forensics records | Hypotheses, evidence, experiments, resolution and workflow post-mortem | debug/forensics agents | resumed debugging/process improvement | Incident-long durable state and report |
| Pause/handoff records (`continue-here`, HANDOFF/CONTINUE) | Exact resumption context | pause/session hooks/custom Julia process | next session/agent | Temporary until safely resumed, but often retained as recovery history |
| Retrospective, milestone archive/audit/summary | Milestone outcomes, history and onboarding synthesis | completion/audit/summary workflows | future milestones/team | Long-lived archive/reference |
| User profile/dev preferences/setup | Behavioral preferences and environment setup needs | profiler/planner/executor | adaptive prompts and human | Cross-session/user or project persistent |
| Capture/backlog/seed/thread/workstream stores | Deferred ideas and persistent parallel/context state | capture and management workflows | later routing/planning | Durable until promoted/completed/removed |
| `.planning/graphs/{graph.json,graph.html,GRAPH_REPORT.md,.last-build-snapshot.json}` | Machine graph, interactive view, narrative report and diff baseline | Graphify build/snapshot | graph query/status/diff, humans and agents | Derived and rebuildable, but snapshot is required for diff; freshness tied to commit and mtime |
| Visual proof files | Screenshots/evidence for UI/boundary verification | UI/UAT/custom Julia protocols | reviewer/human | Per verification event; retained audit evidence |
| Methodology/discovery/spike/sketch artifacts | Capture experimental approach or method when worth retaining | exploration workflows | later spec/planning | Variable: throwaway prototypes vs promoted durable findings |
| Generated runtime manifests/locks/ledgers | Installed file hashes, capability activation/trust/consent/source/state, command roster, config defaults/schema, model catalog | build/installer/capability manager | installer, surface manager, CLI, hooks | Runtime/package state; generated or machine-maintained, not project narrative authority |

Julia contains live examples of nearly every major lifecycle layer: root planning authority files, many phase contexts/UATs/audits, codebase maps, debug and phase directories, quick tasks, milestone archives, visual proof, pipeline/handoff state, learning.md, and generated Graphify outputs.

## 4. Scripts, hooks, workflows, and integrations

| Mechanism | Function | Producer / trigger | Consumer | Dependencies and lifecycle |
|---|---|---|---|---|
| gsd-core installer | Detect runtime, project/local/global scope; convert/copy commands, skills, agents, hooks and instructions; manage permissions, manifests, migrations and uninstall preservation | npm binary/manual install/update | Runtime agents and users | Node >=22, filesystem/runtime homes; package lifecycle |
| `gsd-tools` / `gsd_run` | Structured artifact queries/mutations, validation, phase/state/config/Graphify operations, portable path discovery | Commands/workflows/hooks | Agents, scripts and tests | Node runtime plus `.planning/`; active throughout project |
| Context monitor/session state/statusline | Warn on context usage, expose phase lifecycle and state, record session activity | tool/session hooks | Active agent/user | Runtime hook lifecycle; reads config and STATE |
| Read/prompt/injection guards | Enforce read-before-edit, scan untrusted content/prompt injection and block unsafe patterns | pre/post-tool and prompt hooks | Agent/runtime safety layer | Every relevant tool/prompt event; depends on hook support and security config |
| Workflow/worktree/path guards | Prevent invalid phase transitions, base/worktree mismatch and noncanonical paths | command/tool hooks | Executor and git workflows | Continuous while installed; depends on git and planning state |
| Commit/phase-boundary validation | Require planning consistency and validate commits at boundaries | commit hooks and phase-boundary scripts | git and phase workflows | Commit/transition events; depends on shell, git, config/artifacts |
| Graphify update/rebuild hooks | Rebuild or refresh graph as source changes and preserve a valid prior graph on failure | post-tool/commit integration | Graphify artifacts/query | Optional capability; external Graphify CLI, git and enabled config |
| Config reload/update check/banner | Reload changing settings; check upstream package version and optionally show banner | session/runtime hooks/background worker | User/runtime | Installed runtime lifecycle; version check may require network |
| Capability overlay system | Install, activate/deactivate, trust, consent, lock and ledger optional/runtime-specific bundles | capability manager/installer/surface | command roster, agents and hooks | Persistent runtime capability state; package descriptors plus config |
| Julia `.github/workflows/gsd-sync.yml` | Weekly/manual subtree pull from `open-gsd/gsd-core` tag v1.6.0, run full tests, force-push update branch and open PR on drift | GitHub Actions cron/manual dispatch | Julia repository maintainers | Live external integration; Node 22, npm, git subtree, GitHub token, write and PR permissions |
| Julia `.husky/pre-commit` | Five-stage bouncer: lint-staged, full lint, type check, GSD preflight, tests, then bounds/debt/GSD sync/plan checks | Every local commit | Developer/git | Live local integration; Node/npm/test environment and multiple Julia scripts |
| Julia `scripts/gsd-sync-check.js` | If staged `app/` or `lib/` changes exist, require a staged core planning file | Husky pre-commit; npm `gsd:sync-check` | Developer/git | Live; depends on git index and exact `.planning` filenames |
| Julia `scripts/gsd-submit.js` | Sealed submission gate: environment integrity, lint/type checks, emulator boundary checklist, TODO debt registration, precise staging/commit | npm `gsd:submit` or manual invocation | Developer/git | Live custom process adjacent to GSD; depends on Java 21 path, Firebase emulator, npm, git and `issues.md` |
| Julia `gsd/active/*.md` | Task-local operational ledger for active work, findings, plans and verification outside/alongside canonical `.planning` phases | GEMINI instruction auto-initialization and agents | Current task agents/human | Work-item lifecycle; custom adopted convention rather than package template |
| Julia runtime instructions | CLAUDE/GEMINI guidance injects planning authority, codebase docs, learning limits, UAT/concerns and active-task rules | Repository maintainers/installers | Claude/Gemini agents | Persistent repository policy; depends on `.planning` and `gsd/active` artifacts |
| Julia package dependency | `gsd-core` installed from `file:./private-packages/gsd-core`; package lock and npm scripts bind it into application install/test lifecycle | npm install/build/test | Julia source and tooling | Live code dependency, not only process content |
| Julia schema imports | `recipeDbSchema` and `clipperInboxSchema` imported/re-exported from `gsd-core` in validation, Firebase write controller and external-data gateway | Application modules | Runtime validation/write paths | Live compile/runtime coupling to gsd-core exports; application function dependency |
| AI-Stack planning documents | Working document, reconciled plan and reconciliation mapping define migration intent and issue close criteria | Process-improvement planning | Issue 40 and later migration work | Project-level reference/authority for the improvement effort, not gsd-core runtime |

Package-maintainer-only scripts (build generation, lint policy, affected-test selection, mutation/coverage, release changesets/notes, npm integrity, branch protection and secret scans) support developing and publishing gsd-core itself. They are part of the package loss surface but are not consumers of Julia's project planning state unless Julia continues maintaining the vendored fork as a package.

## 5. Retained/adopted work requiring explicit functional tracking

### Graphify

Graphify is live in Julia, not merely present upstream:

- `.planning/config.json` explicitly has `graphify.enabled: true`.
- `.planning/graphs/` contains `graph.json`, `graph.html`, `GRAPH_REPORT.md`, and `.last-build-snapshot.json`.
- The function includes build, query, status, diff, commit-based and mtime freshness, an interactive HTML view, a report, diff snapshotting and update hooks.
- The producer chain is gsd-core command/skill → `gsd-tools` adapter → external `graphify` executable → copy/snapshot into `.planning/graphs/`.
- Consumers are agents/humans doing architecture discovery and change-impact analysis, plus status/diff automation.
- Dependencies include Python/Graphify installation, Node adapter, git history, enabled config, runtime skill surfacing and writable graph directories.

### Learning capture

Two related functions coexist:

- Upstream gsd-core `extract-learnings` produces structured per-phase `LEARNINGS.md` from PLAN, SUMMARY, VERIFICATION, UAT and STATE.
- Julia repository policy maintains `.planning/learning.md`, constrained to 1–2 high-value architectural/API/RCA bullets per phase.

Both preserve information otherwise easy to lose, but they have different granularity, producers and storage models. This census intentionally does not assume they are interchangeable.

### Julia's active-task ledger and bouncer protocol

`gsd/active/*.md`, the Husky bouncer, preflight/submit scripts, boundary-proof requirements, CONCERNS/TODO enforcement, schema compilation/gist sync, and Julia-specific pipeline/handoff files are adopted process work around gsd-core. Some are not upstream package capabilities, but their dependencies on GSD names, artifacts or assumptions make them part of the loss surface.

## 6. Dependency map

- Planning chain: PROJECT/REQUIREMENTS → ROADMAP/STATE → SPEC/CONTEXT/RESEARCH/UI-SPEC/AI-SPEC → PLAN → source commits/SUMMARY → VALIDATION/VERIFICATION/UAT/reviews → milestone audit/archive/learning.
- Runtime chain: package descriptors/templates/workflows → installer/converter → runtime commands/skills/agents/hooks → `gsd-tools` → project `.planning` state.
- Git chain: plans and execution manifests → atomic commits/worktrees → verification/review → PR branch/ship/undo.
- Optional external chain: GitHub, external AI CLIs, browser/Playwright, MemPalace, Graphify, web research and cloud ultraplan.
- Julia-specific chain: repository instructions + `.planning` + `gsd/active` → precommit/preflight/submit gates; vendored package → npm lock/install → application schema imports.

## 7. Census boundaries and follow-up evidence gaps

- This is a raw functional census, so no row is labeled replaced, abandoned or needs-new-home.
- Command, skill and workflow counts overlap by design; they are packaging layers around shared functions.
- Package-maintainer CI/release scripts are grouped rather than enumerated file-by-file because their consumer is the gsd-core package-development lifecycle, not a distinct Julia user workflow.
- Runtime-global installed copies and user-level state outside these two repositories were not inspected. The installer proves those artifact types exist, but not which are currently installed on each machine.
- External services (MemPalace, external AI review CLIs, Graphify executable, GitHub Actions run history) were not invoked; repository configuration establishes their intended dependency, while Julia graph files establish that Graphify has been run.
- A disposition pass should use the function rows, not raw file counts, and separately decide the future representation of each artifact family.
