# Julia Migration — Phase 0 Step Plan (for GitHub)

This is the source material for **Phase 0.1**: the issues you'll create on the migration board. It's structured exactly the way we settled — **each task is its own issue**, and **phase is a field**, not an issue. Work top to bottom.

Two rules that hold everywhere in this plan:
- **Close-on-evidence.** An issue closes only when its "Close when" line is a real, checkable fact — never on "done" or "looks fine." This is the discipline you're dog-fooding on the migration itself.
- **Log-it-don't-spin.** Anything you notice mid-work that needs fixing becomes a new issue on the board and *waits its turn*. You don't derail the current issue to chase it.

---

## Board setup (do this before writing issues)

Create a GitHub **Project** (board) for the migration. Configure:

**Fields**
- `Phase` — single-select: `0.1`, `0.2`, `1`, `2`, `3`, `4`, `5`. This is how you see the roadmap (group the board by Phase) or focus (filter to one phase).
- `Owner` — single-select: `Todd`, `Claude Code`. Who does the work.
- `Status` — `Todo` / `In progress` / `Done` (GitHub's default is fine).

**Labels**
- `migration-task` — this plan's own work (build the monitor, wire CI).
- `product-defect` — Julia bugs (fix NYT clipping). Keep these off the board's migration view so the two kinds of work don't tangle.

Skip Milestones — the `Phase` field does the same grouping more cleanly.

---

## Phase 0.1 — GitHub setup  *(Owner: Todd, hands-on)*

This is your first real GitHub reps. You create these issues; doing so *is* the Phase 1 entry-gate skill.

### 0.1-a — Create the migration Project board
Set up the Project with the three fields and two labels above.
**Close when:** the board exists and you can add an issue, set its Phase + Owner, and see it grouped by Phase.

### 0.1-b — Load phases 1–5 as epic issues
Create one epic issue per phase (see "Phase 1–5 epics" below), each carrying that phase's exit criteria as its close condition. These stay open as headers; their tasks get filled in when each phase is approached.
**Close when:** an epic issue exists for each of 1, 2, 3, 4, 5, each with its exit criteria pasted in as the close condition, tagged with the right Phase field.

### 0.1-c — Load Phase 0.2's task-issues onto the board
Create each 0.2 issue below, Phase = `0.2`, Owner = `Claude Code` (except the last, which is yours).
**Close when:** every 0.2 task in this doc exists as an issue on the board with its close condition in the body.

### 0.1-d — Reps checkpoint (your capability rep)
Not a setup step — a self-check that the skill landed.
**Close when:** you can, unaided, create an issue, set Phase + Owner, write a close condition, and close an issue against stated evidence — without someone walking you through the buttons.

---

## Phase 0.2 — Data baseline  *(Owner: Claude Code reads + Todd judges)*

Read-only. Nothing here writes to production, so there's no Type-2 gate and nothing can be made worse by running it. Claude Code does the retrieval; the evidence comes to you already translated.

### 0.2-a — Define the read surface
Decide exactly what "production" the baseline covers — which Firestore collection(s) — and that it sweeps **every document**, not just the 114 known canonical IDs. The stray docs live *outside* the canonical set, so an ID-limited scan would miss them.
**Close when:** a written scope confirms a whole-collection sweep, not an ID-list check.

### 0.2-b — Confirm the canonical reference
Confirm the Aug 7 byte-identical backup is still the trusted truth to diff against, independent and unmodified.
**Close when:** canonical is confirmed as the reference, and that confirmation is itself re-checkable (not just asserted).

### 0.2-c — Run the fresh read-only audit against live production
Reuse the existing audit scripts to diff every production doc against canonical and enumerate every deviation. Must be **fresh and timestamped**, because production keeps drifting on its own (the 102→90 and 8→23 shifts prove the picture moves).
**Close when:** a timestamped raw enumeration exists of every deviating record with its actual field values.

### 0.2-d — Sort deviations into contamination types
Bucket the deviations — mock-user-123 writes, SHA-256 mismatches (and how far they overlap the owner_uid set), stray/injected docs, fabricated-instruction records, plus anything **new** that appears in no prior doc.
**Close when:** each category is a named bucket and every member is a real ID with a count.

### 0.2-e — Reconcile reality against the old docs
Compare what the audit found against CONCERNS.md / STATE.md and record where they disagree. This is the reason Phase 0 exists — the old docs are self-contradictory and were wrong before (Phase 49 "closed" things that weren't; the owner_uid "vestigial" call was false).
**Close when:** a discrepancy list exists — the receipts justifying not trusting the paperwork.

### 0.2-f — Produce the verified dirt list  *(0.2 mechanical exit)*
Consolidate 0.2-d and 0.2-e into one list where every item is: *what's wrong / which records / the real fact that proves it / when it was read.*
**Close when:** that list exists, each item concrete enough to become a Phase 1 product issue with no further investigation.

### 0.2-g — Your product-judge read of the list  *(0.2 capability exit; Owner: Todd)*
You read the translated list and confirm what's contaminated and why, before it's blessed as the baseline. This is your real judgment pass on Julia's data — distinct from practicing the skill on the sandbox.
**Close when:** you've signed off that the list matches reality as you read it.

> The dirt list is perishable — production drifts — so it's a timestamped snapshot. That's the argument for starting Phase 1 close on its heels rather than letting the baseline age.

---

## Phases 1–5 — epic issues (load now; break into tasks when approached)

Create these as epics in step 0.1-b. Each carries its **exit criteria as the close condition**. Tasks under them get written when you reach that phase — not now.

### Phase 1 — Spine + first trust piece
**Close when:** GitHub Issues is live holding the verified dirt list as structured issues; the integrity monitor runs on a heartbeat against real production **and demonstrably flags the 102 mock-user-123 records** — AND you can author, review, and evidence-gate issues unaided.

### Phase 2 — Enforcement substrate
**Close when:** CI + branch protection block any un-passed merge; pre-commit + lint/types wired; the regression net locks real working paths (Serious Eats clip, raw-paste import), each **proven to go red on a real break** and anchored to real ground-truth — AND you can confirm a test genuinely protects a path. ("Suite is green" is not evidence of anything user-facing.)

### Phase 3 — New dev loop
**Close when:** Codex is confirmed to really act on the repo + run real tests (ground-truth, not claimed); Pocock's skills installed and actually invoked; two-model cross-review proven to run independently; blast-radius opt-in fires mechanically off the diff; spend caps set; **pool discipline demonstrably routing** (a cosmetic change stayed single-model, a flagged-path change correctly escalated); loop observability emits a complete artifact trail — AND you can make the three judgment calls (right-sized-fix, escalation, explanation-completeness) and walk the trail to find a failed step, unaided.

### Phase 4 — Cut over + retire
**Close when:** real work flows through the loop not Antigravity; the "what does Claude Code handle natively" audit done; GSD + Antigravity-Kit scaffolding removed **without breaking real user paths** (each concern proven-replaced before its old piece came out); Antigravity benched; the phase-close retrospective has taken over learning.md's job — AND you run the loop as your actual process and can make the cutover judgment.

### Phase 5 — Drain + grand-regression capstone
**Close when:** the grand-regression discovery pass is complete and every find is a filed issue; enough backlog has run through the finished loop to demonstrate it works end-to-end on real defects; Julia is usable for the paths you care about — AND you've done the product-judge discovery pass and can drive the loop on real backlog unaided. **Stop at usable + demonstrated, not pristine** — logged-but-unfixed dirt is an acceptable exit.

---

## Two standing mechanisms (not phases)

- **Phase-close retrospective.** At each phase close (starting with the migration's own), write what went right/wrong across three lenses — **task** (did the work land), **process** (did the machine run — read the artifact trail), **compute cost** (token/quota spend). Feed it to three consumers: your curriculum, the router's tuning, and process fixes. Simplify if it gets cumbersome.
- **Grand regression** sits post-cutover, right before the drain — a distinct step, its own issue, closing when the discovery pass is complete and every finding is filed.
