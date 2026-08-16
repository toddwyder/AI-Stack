# AI-Stack Process Improvement — Working Document

**Status:** Working document, not a finalized plan  
**Purpose:** Preserve the current state of the process-improvement effort so ChatGPT, Claude, and the PM can work from the same shared understanding without relying on conversation memory.

**Current authority:** This document is the current reconciled process plan. Existing AI-Stack phase/issues and older migration-plan documents are valuable current-state and historical inputs, but they do not override this plan. They will be reconciled against it during the current-state/how pass before new migration work is created.

---

## 1. Why this work exists

The development process must reliably distinguish work that actually satisfies the user requirement from work that merely looks complete.

The failure mode that triggered this effort was repeated false confidence in AI-assisted development: implementation agents claimed fixes were complete and supplied convincing-looking evidence, but UAT showed the requirement had not actually been satisfied. This forced the PM into forensic QA and consumed large amounts of time and model usage.

The core chain under improvement is:

**user need → requirement → specification → acceptance criteria → implementation → observable outcome → evidence → independent verification → UAT**

The process should improve both:

1. **Reliability** — work declared correct is actually correct against the agreed requirement.
2. **Human burden** — the PM should not be forced into routine technical forensics to determine whether an agent is telling the truth.

---

## 2. Governing principles

### Evolutionary improvement, not waterfall design

This is an engineering continuous-improvement program.

We should start with the **minimum viable process and tooling**, run it on real Julia development work, observe the actual friction and failure modes, and then improve it.

The process should ratchet forward:

**current best process → run real work → observe → improve → retain improvements that help**

We should not design an elaborate automated system based primarily on theories about where pain might occur.

### Human-operated first

For the first runs, the PM is effectively the workflow orchestrator.

If a task is repetitive, mechanical, expensive, or error-prone in actual use, it becomes a candidate for automation.

Manual work in v1 is not automatically a defect. It is also useful observational data about what should eventually be automated.

### One source of truth

**GitHub is the only durable source of truth** for product backlog, active work, implementation history, evidence, decisions, and workflow audit history.

Derived or convenience artifacts may exist, but they must not become competing sources of truth.

### Foundational controls are not experimental variables

The minimum process should be evaluated as a working operating model, not as a publishable controlled experiment.

Foundational safeguards are included in the baseline rather than removed merely to isolate their individual contribution. In particular, independent acceptance-criteria/evidence design and independent verification are foundational parts of the minimum process.

Experiment 1 evaluates whether the **whole minimum viable process** produces more trustworthy completed work with less PM forensic burden. Individual components may later be changed or removed based on observed results, but v1 does not require single-variable isolation.

### Preserve evidence, do not replace it with summaries

Summaries may be useful for orientation, but they must not overwrite or erase original information.

Anything transmitted from one agent to another that materially affects the work should be preserved in GitHub so failures can later be reconstructed.

### Evidence must be usable, with provenance

The reviewer should inspect authoritative evidence directly whenever technically possible, but the PM should not have to interpret raw technical artifacts to understand whether a requirement passed.

PM-facing evidence should clearly state:

- what was checked;
- where the evidence came from;
- what the evidence establishes;
- whether the acceptance criterion passed.

Raw evidence and provenance remain preserved for auditability. The translation is an aid to understanding, not a substitute for the source evidence.

### Prove outcomes, not green lights

A successful test command, deployment, mocked test, or agent statement is not automatically proof that the user requirement was satisfied.

Where possible, each acceptance criterion should point to an authoritative source of truth that directly establishes the required outcome.

### Prove critical controls when they are established

A configured safeguard is not automatically proof that the safeguard works. When a critical gate, regression control, monitor, or similar protection is introduced, it should be demonstrated to detect or block the failure it claims to handle.

This is an infrastructure-establishment requirement, not a recurring ceremony for every development item.

### Do not mock what is material to the acceptance criterion

Mocks may be used for unrelated dependencies.

A mock cannot prove behavior whose truth depends on the real staging integration or state being tested.

### PM owns product intent and process decisions

Agents can diagnose, gather evidence, challenge assumptions, and recommend changes.

The PM decides what the user wants and owns changes to the development process.

---

## 3. Repositories and scope

### AI-Stack

AI-Stack contains reusable development-process tooling and knowledge intended to behave essentially the same across future projects.

Possible examples include shared skills, lightweight model instructions, process templates, and other reusable development infrastructure.

AI-Stack should evolve based on evidence from real development work.

### Julia

Julia is the cooking application.

Julia contains application-specific code, tests, integrations, environments, product knowledge, and other project-specific artifacts.

The migration to the new process will require changes in **both AI-Stack and Julia**.

---

## 4. Product backlog model

GitHub becomes the authoritative Julia backlog.

The board is intentionally simple:

**Backlog → Ready → In Process → Done**

Only one item is normally In Process at a time.

### Backlog

For the **current Julia migration**, the existing backlog consists of **defects and specs**, not a collection of half-formed ideas. Those defects and specs are moving to GitHub; only the migration mechanism remains to be determined.

The **AI-Stack backlog** also serves as a holding place for ideas and hypotheses that are worth preserving without committing to implementation. It may contain:

- half-formed process/tooling ideas;
- deferred automation candidates;
- future capabilities;
- process hypotheses;
- things worth preserving so they do not have to remain in the PM's head.

AI-Stack backlog items do **not** need to be fully specified. They move toward Ready only when there is a reason to consider implementing them.

### Ready

Backlog → Ready is the **product-definition gate**.

The PM owns this gate.

A Ready item contains enough product definition that the development process can work on it without reconstructing product intent from old conversations or inventing product decisions.

Depending on the work, this may include:

- a defined vertical slice;
- specification;
- UI mockup or other UX definition where relevant;
- important constraints;
- for a defect, the observed incorrect behavior and expected behavior.

The exact artifact package should remain lightweight until real failures show that Ready is too loose.

### In Process

The PM selects the next item and moves it from Ready to In Process.

That pull is the explicit signal that development begins.

The process does not autonomously prioritize or select product work.

### Done

An item reaches Done only after the approved implementation has reached production and the production smoke test has passed.

---

## 5. Minimum viable end-to-end development process

This is the current **what-level process map**.

The exact tools and implementation mechanisms will be determined in a later how/migration pass. Matt Pocock's skills are part of the intended v1 approach; the how pass determines their exact placement and usage rather than whether they must first win a separate adoption experiment. If they do not work well in practice, the process can pivot.

**Experiment 1 is an operational trial of this whole minimum viable process.** It is not a sequence of isolated experiments intended to prove the individual value of Pocock, the reviewer, or other foundational components.

### Step 1 — Product definition reaches Ready

**Purpose:** ensure agents are not forced to invent product intent.

**Minimum viable framework:**
- GitHub issue/project item contains the necessary product definition.
- PM decides when the item is Ready.

**Not required in v1:**
- automated readiness scoring;
- formal readiness checklists beyond what real failures later justify.

---

### Step 2 — PM pulls one Ready item into In Process

**Purpose:** explicitly identify the work being developed.

**Minimum viable framework:**
- PM moves the selected GitHub item to In Process.
- WIP is normally one item.

**Not required in v1:**
- autonomous work selection;
- workflow orchestration skill merely to start the process.

---

### Step 3 — Independent acceptance criteria and sources of truth

**Purpose:** define what must be true before implementation planning can bias the evidence strategy.

An independent reviewer establishes:

1. the acceptance criteria;
2. an authoritative source of truth for each criterion wherever reasonably possible.

The reviewer determines **what must be observable**, not how the implementation agent should create it.

If product intent is genuinely ambiguous, the PM resolves it.

If the criterion is clear but the required source of truth cannot be observed because of tooling/access limitations, that is a capability/observability blocker rather than permission to use a weaker proxy.

**Minimum viable framework:**
- PM invokes the independent reviewer.
- Reviewer reads the authoritative GitHub work item.
- Acceptance criteria and sources of truth are recorded in GitHub.

**Foundational requirement:**

Independent acceptance-criteria/evidence design is part of the minimum process from the beginning. It is not removed from Experiment 1 to isolate other variables.

---

### Step 4 — Implementation planning

**Purpose:** create an implementation approach that is aimed at satisfying the already-defined outcomes.

Claude Code is the initial default implementation agent.

The implementation plan should be preserved for auditability.

The plan is not the definition of success. Acceptance criteria and authoritative evidence remain the standard.

If implementation direction materially changes, history should be amended rather than rewritten.

**Minimum viable framework:**
- Claude plans against the GitHub issue and reviewer-defined criteria.
- Plan is preserved or linked from GitHub.

**Not required in v1:**
- mandatory independent review of the implementation plan.

If later failures demonstrate that plan review would have prevented them, plan review becomes a candidate process improvement.

---

### Step 5 — Implementation

**Purpose:** produce a candidate implementation.

Claude Code is the default implementation agent for the first experiment.

ChatGPT/Codex may supplement implementation work when necessary, but automated model hot-swapping is not part of v1.

**Minimum viable framework:**
- implementation occurs on normal Git/PR infrastructure;
- meaningful implementation history remains auditable.

---

### Step 6 — Automated checks and staging

**Purpose:** ensure the candidate passes known automated checks and exists in a realistic environment before independent verification and UAT.

GitHub Actions are a hard pre-UAT gate.

Staging must use the real integrations and sources of truth that are material to the acceptance criteria.

**Minimum viable framework:**
- required Actions/checks pass;
- candidate is available on staging;
- reviewer can inspect required staging evidence;
- when a critical gate/control is first established, there is evidence that it actually blocks or detects the failure it claims to handle.

---

### Step 7 — Independent verification

**Purpose:** independently determine whether every acceptance criterion is actually established.

The reviewer should inspect authoritative sources of truth directly whenever technically possible.

The reviewer is skeptical in the falsification sense:

> Try to falsify the claim of completion if the evidence permits it, without inventing requirements outside the agreed acceptance criteria.

The reviewer must not merely grade Claude's evidence package.

Failure to establish a criterion means the review does not pass.

**Minimum viable framework:**
- PM invokes independent reviewer;
- reviewer inspects repo/staging/evidence sources;
- result and evidence references are preserved in GitHub;
- PM-facing output states what was checked, the source/provenance, what the evidence establishes, and whether each criterion passed.

Reviewer approval and UAT must apply to the same implementation candidate.

---

### Step 8 — Guided PM UAT

**Purpose:** final product validation by the PM.

UAT is defined in the GitHub issue and derived from the already-agreed acceptance criteria.

The role of guidance is to make those checks easy for the PM to perform without turning the PM into technical QA.

UAT may remain exploratory.

If UAT exposes a new product preference or previously unstated requirement, that is product learning rather than automatically a verification failure.

If UAT shows that an already-agreed requirement was not satisfied after the development process declared the work ready, that is a false-confidence event and may trigger a retrospective.

**Minimum viable framework:**
- existing UAT steps live in the issue;
- PM performs them on staging;
- result is recorded in GitHub.

**Not required in v1:**
- custom guided-UAT automation. The PM can run the issue's UAT manually with an agent assisting conversationally.

---

### Step 9 — Production deployment

**Purpose:** promote the exact candidate that passed independent verification and UAT.

UAT pass authorizes production deployment.

The exact implementation of deployment is a how/research question.

---

### Step 10 — Production smoke test

**Purpose:** establish that the deployed production system actually works at a basic operational/product level.

A successful deployment mechanism is not itself proof that production is functioning correctly.

**Hard requirement:**
- the production deployment must be followed by a smoke test;
- the work cannot reach Done until the production smoke test passes.

The implementation mechanism may eventually be manual, agent-driven, GitHub Actions-driven, or some combination. That remains a research item.

---

### Step 11 — Production recovery on smoke-test failure

If the production smoke test fails, production must be restored to a known-good state.

The exact recovery/rollback mechanism is a how-phase research item.

---

### Step 12 — Done

Only after the production smoke test passes:

- close/complete the issue;
- move the item to Done.

---

## 6. Failure handling in the manual-first process

We should not build a universal failure engine before experience shows that we need one.

For v1, the minimum failure contract is:

1. collect the relevant facts first;
2. preserve durable evidence/state in GitHub;
3. explain why progress stopped;
4. tell the PM what decision or action is required.

The PM decides the resolution and how the process resumes.

### Existing escalation principles

These remain useful even without automation:

- repeated attempts are acceptable while they produce materially new information;
- repeating the same failure/diagnosis/evidence without learning means stop and change approach;
- two consecutive routine reviewer failures on one issue trigger a mini-retrospective;
- a materially serious reviewer failure should surface immediately;
- rollback #3 is a hard escalation trigger;
- a qualifying UAT failure triggers a mini-retrospective focused on:
  - **Why didn't we catch this before UAT?**
  - **What would have prevented this from reaching UAT?**

Retrospectives can initially be run manually from the GitHub evidence.

---

## 7. Milestone learning

Every milestone ends with a retrospective.

The milestone review should consider:

- reliability;
- preventable UAT failures;
- rework;
- reviewer retries;
- repair cycles;
- rollbacks;
- PM burden;
- investigative/forensic effort;
- cost and quota pressure;
- flow;
- evidence about whether the minimum process is helping.

Experiment 1 is judged operationally, not as a controlled scientific study. The retrospective should decide what to:

- **Keep** — working well enough to retain;
- **Adjust** — useful but needs change;
- **Drop / replace** — not earning its cost or not solving the problem;
- **Continue observing** — insufficient evidence yet.

Real product work generates the data. We do not manufacture work merely to isolate variables or exercise an experiment.

The milestone retrospective should also ask:

> What did we repeatedly have to do manually that was painful, mechanical, expensive, or error-prone enough that automating it would materially improve the process?

That is the primary mechanism for discovering the next automation target.

---

## 8. AI-Stack backlog — ideas deliberately deferred from v1

These ideas must **not be lost** merely because they are not part of the minimum viable process.

They belong in the AI-Stack backlog as possible future improvements/hypotheses.

Current candidates include:

- `/init` workflow kickoff skill;
- skill-to-skill automatic chaining;
- PM-notification/briefing skill;
- `state.md` or another lightweight workflow-state mechanism;
- automatic workflow resumability;
- dedicated guided-UAT skill;
- mini-retro skill;
- milestone-retro skill;
- token-audit skill;
- automatic quota-wall handling;
- automated model switching/hot-swapping;
- context routing/selective loading;
- plan-review gate;
- automated reviewer invocation;
- dedicated deploy skill;
- dedicated smoke-test skill;
- post-deploy smoke testing via GitHub Actions;
- loop/workflow automation;
- more sophisticated orchestration;
- multi-item/concurrent workflow support;
- loops/graphs/agent swarms;
- `learning.md` pruning or retrieval layers if it eventually becomes too large.

These are backlog ideas, not commitments.

Real friction should determine which ones move toward Ready.

---

## 9. gsd-core sunset analysis

Before removing gsd-core, we must explicitly determine what we would lose.

The analysis should map:

**gsd-core capability → whether the new process replaces it → whether we intentionally abandon it → whether the useful capability needs another home**

This must happen before gsd-core is sunset.

One already-identified capability worth retaining is the end-of-phase project-learning artifact, currently conceptualized as Julia's cumulative `learning.md`.

Previously retained gsd-core/tool-adoption work, including Graphify, should be treated as input to this inventory rather than assumed to survive unchanged.

The complete gsd-core inventory still needs to be researched.

---

## 10. Julia project memory

Julia should retain cumulative durable project knowledge that helps future agents work effectively on Julia.

Current working concept:

**`learning.md`**

It may contain:

- architectural quirks;
- integration behavior;
- recurring traps;
- non-obvious constraints;
- important project-specific discoveries.

It is not the audit trail.

GitHub remains the source of truth for historical evidence.

No pruning, retrieval system, archive system, or elaborate knowledge architecture should be built until file size/context usage becomes an observed problem.

---

## 11. Two-model / two-harness collaboration

The project is expected to use both:

- **Claude Code**
- **ChatGPT/Codex**

The likely operating model is two harnesses/models working against the same project environment and authoritative GitHub state.

Claude may eventually act as a lightweight orchestrator and may invoke Codex through a plugin or similar mechanism, but the exact mechanism is not yet decided.

The dependency order for the how-phase is:

1. determine the practical two-harness/two-model operating arrangement;
2. give both environments the required access to GitHub/repositories;
3. ensure the reviewer can access the staging/runtime sources of truth required by the acceptance criteria.

Reviewer independence must be preserved even if Claude initiates the reviewer call.

V1 does **not** require automatic Claude↔Codex orchestration. The minimum acceptable arrangement may simply be the PM manually invoking two independent harnesses that share authoritative GitHub/repository state and can access the evidence required for their roles.

The exact plugin, context-passing mechanism, permissions, and invocation architecture remain research items.

---

## 12. Three-party project collaboration

This project will be worked on by:

- the PM;
- ChatGPT;
- Claude.

Claude needs to be part of the process-improvement conversation rather than being treated only as an implementation worker.

A prior Claude working plan has now been compared against this document and the substantive divergences have been reconciled into this single working plan.

This working document is intended to be the shared briefing artifact. Before the next Claude read-in, the PM will first provide the actual current-state update so this plan can be corrected for anything that has already changed in implementation.

When Claude is brought back in:

1. give Claude the updated single-source working document;
2. explain any current-state corrections made after the PM read-in;
3. ask Claude to challenge remaining assumptions, especially:
   - missing operational constraints;
   - useful gsd-core capabilities at risk of being lost;
   - assumptions about Claude Code that are inaccurate;
   - likely integration issues in a two-harness environment;
4. preserve Claude's substantive feedback in the shared project record;
5. reconcile disagreements explicitly rather than silently merging perspectives.

The goal is not for Claude or ChatGPT to own the process.

The three parties should contribute different perspectives while the PM remains the decision-maker.

---

## 13. Migration/how phase — required next pass

The current document primarily defines **what must happen**.

Before Julia development resumes, we need a separate **how/current-state pass**.

Before that pass is treated as authoritative, the PM will provide a current-state read-in so work already completed, partially completed, or changed is not accidentally planned again.

Importantly, the how pass should walk the same development process end-to-end rather than becoming a disconnected technology architecture exercise.

For each process step, ask:

1. What must happen?
2. Do we really need this step?
3. What already exists?
4. What is the minimum viable mechanism to make it work?
5. What does gsd-core currently contribute here?
6. How should the adopted Pocock skill/capability be used here, and what gap remains after using it?
7. What deferred automation idea is relevant but unnecessary for v1?
8. What gap remains?
9. Which repo does that gap belong in?
10. What GitHub migration issue is required?

### Known how-phase research areas

#### A. Two-harness / two-model operating arrangement

Determine how Claude Code and ChatGPT/Codex will practically work against the same project.

This comes before detailed reviewer-access design.

#### B. GitHub/repository/environment access

Once the two-harness model is understood, give both environments the access needed for their roles.

#### C. Pocock skills integration

Matt Pocock's skills are adopted for the minimum process. The how pass determines which skills map to which process steps, how they actually operate in our harnesses, and what gaps remain.

Avoid rebuilding capabilities the skills already provide. If real use shows that the approach does not work for this operating model, pivot rather than preserving it for the sake of the plan.

#### D. gsd-core capability/loss inventory

Map everything useful that would disappear when gsd-core is removed.

#### E. Julia current-state audit

Determine what Julia currently has for:

- GitHub/project backlog;
- CI/GitHub Actions;
- tests, including integration-test coverage;
- logging/observability;
- staging;
- real integrations;
- deployment;
- rollback/recovery;
- production smoke testing;
- repository/model instructions;
- project memory;
- relevant access and observability.

#### F. Julia backlog migration

The Julia backlog being migrated consists of **defects and specs**.

The end state is settled: GitHub Issues/Projects becomes the authoritative Julia backlog.

The migration mechanism is intentionally unresolved. Research the best way to move the existing defects/specs into GitHub, including:

- direct/scripted issue creation;
- using the existing material as input to a `to-spec` process;
- a hybrid approach.

Any previously built migration tooling reported in earlier work (for example, a bulk issue-creation script) should be verified during the current-state read-in and evaluated as an available mechanism, not assumed to be the final answer.

#### G. Julia data-integrity safety control

Julia currently has known corrupted recipes whose root cause has not yet been established. Until the underlying failure is understood and adequately prevented/observed, Julia needs a way to detect new corruption attempts or occurrences.

This integrity monitor is a **Julia-specific migration/safety requirement**, not a global AI-Stack process capability.

The how pass should determine the minimum viable monitor plus the underlying integration-test and logging/observability improvements needed to make the special monitor unnecessary once the root cause is understood and controlled.

When the monitor is established, prove that it can detect the known corruption pattern rather than merely proving that the monitor is configured.

#### H. Production smoke test and recovery

A production smoke test is required.

Research the minimum viable implementation and recovery mechanism.

#### I. Grand Regression — Julia defect-discovery pass

Julia currently has many defects that were never systematically logged because development had fallen into a whack-a-mole pattern: fixing one defect often exposed another, so the backlog never became a complete inventory of product problems.

**Grand Regression** is the PM-led full-product discovery pass that corrects that. The PM goes through the whole Julia application and real user paths and logs every defect found into the authoritative GitHub backlog.

Grand Regression is **not** a regression-test suite, a generic AI-Stack automation, or a prerequisite for defining the minimum development process. It is a Julia backlog-discovery activity that should run once the minimum new process is operational, so the newly discovered defects can flow through that process rather than through the process being retired.

Completion of the initial Julia backlog migration therefore means that all **currently documented defects and specs** have moved to GitHub; it does not claim that Julia's defect inventory is complete. Grand Regression is the later systematic pass that builds that fuller defect inventory.

---

## 14. Migration planning rule

The PM's immediate current-state read-in comes first so the working document does not plan from stale assumptions. After the future process and how-level mechanisms are understood, perform the **full current-state capability audit** needed to calculate the implementation delta.

The migration plan is:

**required future operating capability − actual current capability = migration work**

Migration work may affect both AI-Stack and Julia.

The resulting work becomes GitHub issues in the repository that owns the thing being changed: reusable AI-Stack work in AI-Stack; Julia-specific work in Julia. Do not require an inherited Phase → decimal issue → lettered sub-issue hierarchy merely because previous migration work used one.

Whether related issues are grouped into one milestone or several is an implementation detail decided after the work is understood.

Normal Julia feature development remains frozen until the minimum migration required to run the **whole end-to-end process** is complete. Julia is then the real-work proving ground for that process. We do not reopen Julia piecemeal merely to prove individual AI-Stack artifacts.

---

## 15. What is settled versus intentionally unresolved

### Settled

- GitHub is the single durable source of truth.
- GitHub becomes the authoritative Julia backlog.
- The Julia backlog being migrated consists of defects and specs; the migration mechanism is a how-phase question.
- Board states are Backlog → Ready → In Process → Done.
- The AI-Stack backlog may hold half-formed ideas, deferred automation candidates, and process hypotheses without commitment to implement them.
- PM owns Backlog → Ready.
- PM chooses what moves Ready → In Process.
- WIP is normally one item.
- Independent acceptance-criteria/evidence design and independent verification are foundational parts of the minimum process, not experimental variables.
- Experiment 1 evaluates the **whole minimum viable process** on real Julia work rather than isolating individual variables.
- Matt Pocock's skills are adopted for v1; exact usage is determined in the how pass and can be changed if real use shows they do not work.
- Claude Code is the default first implementation agent.
- ChatGPT/Codex is the default independent reviewer.
- GitHub Actions are a hard pre-UAT gate.
- Staging must use real integrations when they are material to the acceptance criterion.
- Critical safeguards should be demonstrated to detect/block the claimed failure when first established, but this is not a recurring ceremony for every item.
- Reviewer approval precedes PM UAT.
- Reviewer output to the PM must include evidence provenance and a plain explanation of what was checked, what the evidence proves, and whether each criterion passed.
- UAT lives in the GitHub issue.
- UAT is product validation, not routine forensic QA.
- The exact candidate reviewed must be the candidate used for UAT.
- UAT pass authorizes production deployment.
- Production deployment must be followed by a production smoke test.
- Failed production smoke requires recovery to a known-good state.
- Done occurs only after successful production smoke.
- The full lifecycle remains: independent verification → PM UAT → deploy → production smoke → recovery if needed → Done.
- Manual orchestration is preferred initially.
- The two-harness/two-model operating arrangement is an up-front how-phase dependency, but v1 does not require automatic Claude↔Codex orchestration.
- Automation should emerge from observed pain.
- Loop/workflow automation belongs in the AI-Stack backlog rather than being a precommitted experiment/build step.
- Deferred automation ideas belong in the AI-Stack backlog rather than being discarded.
- Julia's integrity monitor is retained as a Julia-specific temporary safety control while recipe corruption remains unexplained; it is not a global AI-Stack capability.
- gsd-core must be fully inventoried before sunset, including previously retained capabilities/tool-adoption work such as Graphify.
- Migration issues live in the repository that owns the change; no mandatory inherited phase/decimal/sub-issue hierarchy is required.
- Julia remains frozen until the minimum migration needed to run the whole end-to-end process is complete; it will not reopen piecemeal to prove individual AI-Stack artifacts.
- Grand Regression is a PM-led full-Julia defect-discovery pass performed after the minimum new process is operational; it is not a regression-test suite or global AI-Stack capability.
- "Julia backlog migrated" means the currently documented defects/specs are in GitHub, not that Julia's full defect inventory is complete; Grand Regression later discovers and logs the rest.
- Claude is a collaborator in the process-improvement project, and substantive disagreements are reconciled explicitly.

### Intentionally unresolved

- actual current state of AI-Stack and Julia, pending the PM current-state read-in;
- exact Pocock skill usage/integration;
- exact Julia backlog migration method: script, `to-spec`, or hybrid;
- whether previously reported migration tooling remains useful after current-state verification;
- exact Claude/Codex plugin or invocation mechanism;
- exact two-harness architecture;
- exact model permissions/access configuration;
- exact staging/reviewer observability mechanism;
- exact Julia integrity-monitor implementation and the conditions for retiring it;
- exact integration-test/logging improvements needed around the Julia corruption problem;
- exact production deployment mechanism;
- exact production smoke-test mechanism;
- exact rollback/recovery mechanism;
- which deferred process steps eventually deserve custom skills;
- whether a lightweight `state.md` becomes useful after actual workflow experience;
- when/if more sophisticated orchestration, loops, graphs, or concurrency become justified.

---

## 16. Immediate next actions

Before creating new migration issues or treating earlier migration state as authoritative:

1. Inspect **AI-Stack issues and Julia repository artifacts directly in GitHub** to establish the evidence-based current state; the PM supplies context, corrections, and facts GitHub cannot establish.
2. Reconcile that current state against this working plan and change the plan if reality requires it.
3. **Share this reconciled working document with Claude** and bring Claude up to speed on the decisions and current-state corrections.
4. Run the **how/current-state pass** end-to-end, including the practical two-harness/two-model operating model.
5. Inventory what would be lost by sunsetting **gsd-core**.
6. Determine how the adopted **Pocock skills** fit each relevant process step.
7. Resolve the Julia backlog migration mechanism: **script vs. `to-spec` vs. hybrid**.
8. Audit Julia's tests, integration tests, logging/observability, staging, deployment, smoke testing, recovery, and the temporary integrity-monitor need.
9. Determine the concrete migration delta.
10. Create migration issues in the repository that owns each change.
11. Complete the minimum migration required to run the whole end-to-end process.
12. Unfreeze Julia and run Experiment 1 manually on real work.
13. Use actual reliability, UAT, rework, PM-burden, cost, and friction data to decide what to keep, adjust, drop, or automate next.

---

## 17. Working-document rule

This document is intentionally not final.

Update it when decisions change or new evidence changes the process.

Do not silently delete deferred ideas or past decisions simply because they are no longer part of the MVP. Move them to the appropriate backlog/history section so the reasoning remains recoverable.

The objective is to keep the project evolvable without forcing the PM, ChatGPT, or Claude to reconstruct the project from conversation memory.
