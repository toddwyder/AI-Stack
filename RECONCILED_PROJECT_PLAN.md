# AI-Stack Process Improvement — Structured Project Plan

**Derived from:** `PROCESS_IMPROVEMENT_WORKING_DOCUMENT.md` (AI-Stack, main, committed ~37 min
before this draft)
**Purpose:** give the working document a milestone/issue shape so it can be reconciled,
item-by-item, against the 23 legacy Phase-hierarchy AI-Stack issues per §14's rule. Nothing
below adds scope the source document doesn't already commit to. Where the source explicitly
leaves something as a "how-phase research item," that's marked **UNSCOPED** rather than given
an invented close condition.

Per §14: *"Do not require an inherited Phase → decimal issue → lettered sub-issue hierarchy
merely because previous migration work used one. Whether related issues are grouped into one
milestone or several is an implementation detail decided after the work is understood."*
The milestone groupings below are a proposed grouping for this reconciliation pass, not a
reinstated hierarchy — open to collapsing/splitting once reconciled against the old issues.

Repo assignment follows §3/§14: reusable → AI-Stack, Julia-specific → Julia. Marked per item.

**Governing principles:** see source §2 for the 10 principles (close-on-evidence,
human-operated-first, one source of truth, foundational-controls-not-variables,
preserve-evidence, evidence-with-provenance, prove-outcomes-not-green-lights,
prove-critical-controls, don't-mock-what's-material, PM-owns-intent). Not restated here to
avoid a second copy that can drift from §2 — every close condition below is written to
satisfy them. Called out inline only where a principle changes how a specific close condition
must be judged.

---

## Milestone A — Reconcile legacy Phase-hierarchy issues (§14, Discussion #32)

This milestone is procedurally first: it's the mechanism for retiring the 23 old issues we're
about to reconcile against, so it has to run *as* the reconciliation, not after it.

**Repo:** AI-Stack

| # | Issue | Close when |
|---|---|---|
| A1 | Todd + Claude review each of the 23 legacy issues; check whether its underlying idea is already represented in the working document | Every one of the 23 has a documented disposition: represented / needs a new home / obsolete |
| A2 | Write the old-issue → new-home mapping as its own artifact | Mapping file exists, one row per legacy issue, target = working-doc section, AI-Stack backlog (§8), or "superseded, no replacement" |
| A3 | Claude Code executes mechanical GitHub changes from the mapping (create/update items, add superseded pointers, close old issues) via script, dry-run gate first | Dry-run reviewed and approved; real run completes; no judgment made outside the mapping |
| A4 | Post-change verification against the mapping | Every old issue accounted for; every retained idea present in its new home; no legacy Phase-hierarchy issue left open by accident |

*Note: this milestone's own procedure (A1–A4) is this document's §14 execution sequence,
already settled — not itself up for renegotiation, just execution.*

*Principle callout (PM owns product intent and process decisions, §2): A1 is judgment-and-
evidence work Todd + Claude do together, not a decision Claude makes unilaterally. A3's script
performs zero judgment — every disposition must already be explicit in the A2 mapping before
it runs.*

---

## Milestone B — Current-state read-in (§12, §13 intro, §16.1–2)

**Repo:** N/A (informational/PM-driven; findings route into Milestone A/C/D)

| # | Issue | Close when |
|---|---|---|
| B1 | PM supplies current-state facts for AI-Stack and Julia directly from GitHub (issues, PRs, CI, tests, staging, deploy) | Read-in complete; corrections captured |
| B2 | Reconcile that current state against the working document; amend the document where reality requires it | Working doc updated or confirmed unchanged, with corrections logged |
| B3 | Full §12 three-party protocol: (1) give Claude the updated single-source document, (2) explain current-state corrections made after PM read-in, (3) ask Claude to challenge remaining assumptions — missing operational constraints, at-risk gsd-core capabilities, inaccurate assumptions about Claude Code, likely two-harness integration issues, (4) preserve Claude's substantive feedback in the shared project record, (5) reconcile disagreements explicitly rather than silently merging perspectives | All 5 steps done; Claude's feedback is preserved in GitHub (not just said in chat); any disagreement has an explicit recorded resolution, not a silent merge |

---

## Milestone C — How-phase design pass (§13, research areas A–I)

**Repo:** AI-Stack (these are process-design issues; their *outputs* may spawn Julia issues
in Milestone D). Each row is a scoping issue in its own right — the deliverable is an answer,
recorded in GitHub, not code. Every issue below gets walked through the §13 ten-question
framework — (1) what must happen, (2) do we need this step, (3) what exists, (4) minimum
viable mechanism, (5) gsd-core's current contribution, (6) Pocock skill fit, (7) relevant
deferred-automation idea, (8) remaining gap, (9) owning repo, (10) resulting migration issue —
and that framework's ten answers, recorded against the issue, ARE its close condition.

| # | Issue | Depends on | Close when |
|---|---|---|---|
| C-A | Scope the two-harness/two-model operating arrangement: how Claude Code + Codex practically work against the same project | none (dependency-order #1 per §11) | Ten-question framework answered; concrete arrangement documented in GitHub |
| C-B | Scope GitHub/repo/environment access each harness needs | C-A | Ten-question framework answered; access requirements documented |
| C-C | Scope Pocock skills integration: which skill maps to which of the 12 process steps (§5), what gaps remain after using each | none | Mapping exists step-by-step; each gap named |
| C-D | Build the gsd-core capability/loss inventory, including previously retained tool-adoption work (Graphify) | none | Inventory complete: capability → replaced-by-new-process / intentionally-abandoned / needs-new-home, for every gsd-core capability |
| C-E | Audit Julia's current state: GitHub/project backlog, CI/Actions, tests incl. integration coverage, logging/observability, staging, real integrations, deployment, rollback/recovery, production smoke testing, repo/model instructions, project memory, access | Milestone B read-in | Audit document exists covering all listed areas with evidence, not assertion |
| C-F1 | Define the Ready-to-pull contract: what a Julia issue/spec must contain to be genuinely pullable into In Process without reconstructing product intent (Discussion #30) | none | Contract documented — candidate shape (observed/expected behavior, enough context for Step 3 acceptance criteria, source-of-truth pointers) confirmed or revised |
| C-F2 | Choose the Julia backlog migration mechanism (script vs. to-spec vs. to-tickets vs. hybrid) against the C-F1 contract; verify previously-built tooling (`bulk_issue_create.py`) against it rather than assuming it's final | C-F1 | Mechanism chosen with the contract as the explicit evaluation standard; existing tooling's fit (or gap) documented |
| C-G1 | Scope the minimum viable Julia data-integrity monitor and the conditions under which it can be retired once root cause is controlled | C-E | Monitor design documented; retirement conditions stated as checkable facts, not "when it feels safe" |
| C-G2 | Scope the underlying integration-test and logging/observability improvements needed to make the monitor unnecessary once the corruption root cause is understood and controlled (§13.G, distinct from the monitor itself) | C-E, C-G1 | Improvements documented as a concrete gap list, not folded silently into C-G1 |
| C-OBS | Scope the staging/reviewer observability mechanism: how the independent reviewer inspects the real staging integrations/sources of truth required by acceptance criteria (§15 unresolved) | C-A, C-B, C-E | Mechanism documented; reviewer's actual access path to each source-of-truth type is concrete, not assumed |
| C-DEPLOY | Scope the production deployment mechanism: how the exact candidate that passed verification + UAT gets promoted to production (§5 Step 9, §15 unresolved — distinct from the smoke test that follows it) | C-E | Mechanism documented |
| C-H | Scope the minimum viable production smoke-test and rollback/recovery mechanism | C-DEPLOY | Mechanism documented; what "known-good state" means for Julia is concrete |
| C-I | Grand Regression is explicitly **not** scoped here — it's a post-Milestone-D activity (PM-led discovery pass), not a design question. No issue in this milestone. | — | N/A — tracked as a trigger condition in Milestone E, not a Milestone C deliverable |

Every C-issue's answer is itself an input to Milestone D (D1's delta calculation) — Milestone C
doesn't close by "feeling done," it closes when each row's documented answer exists in GitHub.

*Principle callout (prove critical controls when established, §2): C-G1 and C-H don't close on
"the monitor/smoke-test exists" — each needs a demonstrated-detection step (monitor actually
flags the known mock-user-123 pattern; smoke test actually catches a real deployment problem)
before it counts as scoped-and-proven, not just scoped-and-configured.*

---

## Milestone D — Migration execution (§14, §16.9–11)

**Repo:** split per resulting issue, decided at creation time per §14's rule.

| # | Issue | Close when |
|---|---|---|
| D0 | Create the Julia product-backlog board: `Backlog → Ready → In Process → Done` (§4) — the settled board shape, not a design question, so it doesn't belong in Milestone C | Board exists with the four states; PM can add an item, move it through each state, and WIP-limit In Process to one |
| D1 | Calculate migration delta: required future capability − actual current capability (output of Milestones B + C) | Delta documented |
| D2 | Create migration issues in the repo that owns each change (no mandatory phase/decimal hierarchy) | Every delta item has a corresponding issue in the correct repo |
| D3 | Complete the minimum migration needed to run the whole end-to-end 12-step process (§5) | All required migration issues closed on evidence |

---

## Milestone E — Experiment 1: run the process on real work (§5, §7, §8, §16.12–13)

**Repo:** Julia (execution), AI-Stack (retrospective outputs / backlog additions)

| # | Issue | Close when |
|---|---|---|
| E1 | Unfreeze Julia; PM pulls first Ready item into In Process | One real item In Process |
| E2 | Run the 12-step process end-to-end on that item (Steps 1–12, §5) | Item reaches Done (production smoke test passed) |
| E3 | Record primary failure/success outcome per item at the PM/UAT handoff boundary (§15 locked metric) | Outcome recorded per item, not rationalized after the fact |
| E4 | Milestone retrospective: reliability, rework, PM burden, cost, flow → Keep / Adjust / Drop / Continue-observing | Retrospective written; decision recorded; new automation candidates (if any) added to AI-Stack backlog (§8) |

*E1–E4 repeats per milestone going forward — this is the standing operating loop the whole
document exists to establish, not a one-time step.*

*Principle callout (prove outcomes not green lights, §2): E2/E3's evidence must trace to
authoritative sources of truth, never to a passing check, a mocked test, or an agent's own
say-so — a green Action run is a developer-side signal only and never counted as user-facing
proof at the E3 handoff.*

---

## Standing mechanisms (not milestone-bound — run continuously per §6, §7, §9–11)

- **Failure handling contract** (§6): collect facts → preserve evidence in GitHub → explain
  why progress stopped → tell PM what decision is needed. Escalation triggers (2 consecutive
  reviewer failures, serious reviewer failure, rollback #3, qualifying UAT failure) apply
  throughout Milestone E and beyond.
- **Harness interruption / checkpoint recovery** (§11): resume from last durable checkpoint,
  never from a harness's partial/inferred state; item stays In Process through interruption.
- **Julia project memory (`learning.md`)** (§10): cumulative durable knowledge, not the audit
  trail; no pruning/retrieval system until size becomes an observed problem.
- **gsd-core sunset** (§9): only proceeds once Milestone C-D's inventory is complete and
  Milestone D has replaced what's being retired.

---

## AI-Stack backlog — deferred, not scheduled (§8)

Not part of any milestone above. Listed here for visibility only; items move to Milestone C/D
scope only when real friction justifies it (per §7's "what did we repeatedly have to do
manually" test):

`/init` kickoff skill · skill-to-skill chaining · PM-notification/briefing skill · `state.md`
mechanism · automatic workflow resumability · guided-UAT skill · mini-retro skill ·
milestone-retro skill · token-audit skill · automatic quota-wall handling · automated model
hot-swapping · context routing/selective loading · plan-review gate · automated reviewer
invocation · deploy skill · smoke-test skill · post-deploy smoke via Actions · loop/workflow
automation · multi-item/concurrent workflow support · loops/graphs/agent swarms ·
`learning.md` pruning/retrieval.

---

## What this plan deliberately does NOT do

- It does not invent close conditions for the *answers* the source document leaves open
  (§15 "Intentionally unresolved" list) — but each open question is itself now a scoping
  issue in Milestone C with a real close condition (documented answer in GitHub), not a
  parked "unscoped" label.
- It does not reinstate the Phase → decimal → sub-issue numbering the source document retired.
- It does not treat Grand Regression, gsd-core sunset, or AI-Stack backlog ideas as scheduled
  work — all three are explicitly gated on later conditions in the source document.
- It does not restate the 10 governing principles (§2) as a second copy — they're pointed to
  once, near the top, and called out inline only where a principle changes how a specific
  close condition must be judged.

**Completeness-check corrections (this pass):** added D0 (board setup, §4 — previously
missing entirely), C-DEPLOY (production deployment mechanism, §5/§15 — previously missing),
C-OBS (staging/reviewer observability mechanism, §15 — previously missing), split C-G into
C-G1 (monitor) and C-G2 (integration-test/logging improvements, §13.G — previously folded in
and lost), and expanded B3 to the full 5-step §12 protocol (previously only steps 3–4).
