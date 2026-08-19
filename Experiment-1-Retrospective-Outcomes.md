# Experiment 1 Retrospective Outcomes

These are the operating changes agreed during the Experiment 1 retrospective. They are a starting point: measure their burden and effectiveness, then revise from observed evidence.

## Start Doing

1. **Use deterministic, evidence-first contracts.** Declare the claim, acceptance criteria, review class, scope, and seam IDs before work starts. A seam trace—not a diff alone—must show that L0 crossed no seams or L1 crossed exactly its declared seam.

2. **Instrument before repair.** Establish a diagnostic baseline before selecting a repair when the cause is not already demonstrated. Build a shared telemetry standard so each task uses stable event/seam IDs and a repeatable diagnostic baseline rather than inventing one-off logging.

3. **Initialize every step with a What–How–Stop contract.**
   - **What:** the one outcome for the step.
   - **How:** designated model and effort, GitHub input manifest, telemetry/evidence requirements, review class, budget, and deliverable.
   - **Stop:** prohibited actions/artifacts and the narrow conditions that require a PM decision.
   The agent begins with a high-level readback and asks the essential questions before planning or execution.

4. **Rewrite role skills around What–How–Stop.** Each skill must name its fixed model/effort, exact GitHub inputs, required output format, and stop rules. Initially the PM will initialize steps; later a hook should invoke the same proven initializer.

5. **Use a fixed, incremental evidence packet.** Create the packet at step start and populate it as work occurs. Before review, automatically preflight its completeness. It must include the approved contract/version, diagnostic baseline, acceptance evidence, seam trace, full test results (including failures), the compare/diff URL, changed-file mapping, conclusion/limits, and review record.

6. **Use GitHub-only operational handoffs.** Every downstream role receives its authoritative inputs from GitHub: the current issue, approved artifact/version, prior-stage output, and PM decisions. Missing or mismatched artifacts must be flagged; agents must not reconstruct them from chat or memory.

7. **Preserve approved plans automatically.** Approval creates an immutable, versioned GitHub artifact. Any change creates a new version and returns the work to approval. Handoffs, evidence packets, and reviews reference the exact version/hash.

8. **Use structured status taxonomy.** Every acceptance claim is marked as: not attempted, blocked before attempt, void dependency attempt, observed failure, observed pass, or inconclusive. A void dependency attempt is never represented as a product failure or pass.

9. **Use plan-blind adversarial review.** The adversarial reviewer receives the approved contract and evidence packet, not the implementation plan or developer rationale. It must attest that it did not read forbidden artifacts; accidental exposure stops the review and requires a fresh reviewer.

10. **Cap reviews at two rounds.** A third plan or implementation review requires explicit PM approval after stating the unresolved claim, expected incremental value, quota/time cost, and alternatives.

11. **Track capacity as project telemetry.** Track model quota, context-window headroom, work budget, actual versus forecast consumption, and the effectiveness of reduction strategies. See [issue #51](https://github.com/toddwyder/AI-Stack/issues/51).

12. **Split blocked work into new issues.** If work is genuinely separable, create a smaller issue with a new contract and start it from the beginning. If it cannot be split, stop the blocked issue entirely.

## Stop Doing

1. **Stop treating a discovered defect as permission to repair it.** A finding may be necessary to complete the work, but it is a potential contract change. Log it and ask the PM to authorize a new issue, split, or rescope before testing or repairing it.

2. **Stop work on an issue once it is blocked.** Do not continue coding, testing, evidence collection, or review inside that issue. Record the blocker and affected claims, then split, rescope, or wait.

3. **Stop starting work or review from chat history, local notes, or reconstructed context.** If required GitHub artifacts are absent, flag the missing input and do not proceed.

4. **Stop submitting incomplete packets for review.** The reviewer should not spend quota locating diffs, tests, missing evidence, or unrecorded limitations.

5. **Stop automatically spending quota on further review rounds.** After two rounds, pause for a PM decision. Do not review a blocked state; preserve its diagnostic evidence and defer review until there is a stable, unblocked acceptance candidate.

6. **Stop allowing role bleed.** The translator/advisor does not verify evidence or approve work; it turns supplied, approved inputs and PM decisions into a plain-language, one-item developer response.

7. **Stop allowing plan-blind reviewers to consult the plan or prior rationale.** If this occurs, the review is contaminated and must be restarted with a fresh reviewer.

8. **Stop using ambiguous test language.** Do not call an unreachable or unmet dependency a product test failure, and do not imply acceptance from incomplete or inconclusive evidence.

9. **Stop changing model or effort mid-step to compensate for context or quota pressure.** Finish the current atomic item, write the GitHub handoff, and initialize a new step with the chosen settings.

## Continue Doing

1. **Continue using adversarial, evidence-first thinking.** It exposed latent material-use and parser defects that plausible reasoning alone missed.

2. **Continue treating proposed repairs as hypotheses until measurements support them.** Diagnostic logging in Experiment 1 exposed additional defects and disproved plausible fixes before more implementation effort was spent.

3. **Continue recording newly discovered defects.** Findings are valuable project knowledge even when they are out of scope for the current issue.

4. **Continue keeping issues truthfully open and blocked when acceptance is not possible.** Meaningful partial progress does not justify a success claim.

5. **Continue involving the PM for material scope, cost, and contract decisions.** Front-load necessary questions during intake; once the PM confirms the contract, execute within its declared boundaries.

6. **Continue using GitHub as the source of truth.** The change is to make this dependency explicit and enforce it at every role transition.

7. **Continue revising process from evidence.** Treat these outcomes and the evidence-packet template as baselines; retain, simplify, or change them based on observed burden and effectiveness.
