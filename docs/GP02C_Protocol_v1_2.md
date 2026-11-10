# GP02C-01 — Evidence-Freshness Contract and Experimental Hypotheses

**Study:** *When Passing Tests Become Stale: Evidence Freshness in Gemma 4 Software Repair Agents*  
**Protocol:** GP02C-01 v1.2 — revised design and CPU acceptance contract; no live agent intervention  
**Date:** 7 November 2026  
**Status:** Design revised; GP02C-02 v1.2 deterministic CPU fixtures passed; live integration and 36-task evaluation not started

## 1. Decision and scope

GP02B v2.7 is retained as the **frozen development baseline** for designing Conditions A, B and C. Six development tasks have been run. Their reported 3/6 **PATCH_VERIFIED / delivered-and-verified** status requires explicit submission as well as independent evaluation; this must not be equated with protected correctness of every final workspace patch. This is a baseline observation, **not** evidence that any freshness intervention helps. Earlier holds, failed repairs and all raw traces are preserved. Coding-agent improvements from those traces are tracked separately and are **not** incorporated into the research arms.

The 36 previously designated evaluation tasks remain held out. They must not be used to tune, debug or select the freshness intervention. No Gemma/GPU calls or notebook changes are authorised by this document.

### Primary research question

> At fixed task, model, tools and execution budget, does continuously tracking and revalidating test evidence after code changes improve independently verified repair success **beyond** a controller that already requires fresh testing of the final submitted code?

**Primary comparison: Condition C minus Condition B.** A is descriptive context. The paper must remain reportable if C equals or underperforms B.

## 2. What counts as stale evidence?

**Boundary of source assurance:** Matching hashes demonstrate agreement **at test start and test end only**. They cannot establish that the live workspace was never modified between those observations. Real selected checks must run against an **isolated immutable source snapshot** (or verified equivalent). The CPU notebook remains a deterministic contract simulator until GP02C-03 implements that assurance.

A visible-test receipt is **current** only when all of the following hold:

1. The test completed with a valid result against one identifiable source revision.
2. The code-state fingerprint **at test start equals the code-state fingerprint at test end** (the test did not mutate the relevant source).
3. The receipt's code revision and fingerprint match the **present** relevant workspace state.
4. The environment fingerprint, test selection/definition and command identity match those recorded for the test.
5. The test actually executed and collected at least one test; a zero exit status with no collected tests is not a valid passing receipt.
6. The result is not superseded by a later execution of the same identified check.

A passing result obtained at revision **r1** is marked **STALE** when a later relevant edit creates revision **r2**, even if the test would still pass. A real **REGRESSION** requires an actual fresh rerun at r2 to fail. An old result is not automatically incorrect; its applicability is unproven.

**The primary mechanism tracks visible test-execution evidence, not the truth of model reasoning or the semantic adequacy of the tests.** Model context-compaction summaries, stale code-graph snapshots, missing API compatibility and mismatched hidden-test wording are distinct phenomena; record them as limitations or separate exploratory observations, not as proof of this hypothesis.

### Example

| Event | Revision | Receipt | Interpretation |
|---|---|---|---|
| `pytest check_X` passes | r1 | X:r1 = PASS | Current pass for r1 |
| Agent edits implementation | r2 | X:r1 = STALE | Unknown outcome at r2, **not** an observed regression |
| `pytest check_X` passes again | r2 | X:r2 = PASS | Current pass for r2; no regression |
| *Alternative:* `pytest check_X` fails | r2 | X:r2 = FAIL | Observed regression / failed current check |

## 3. Evidence identity and ledger schema

A test receipt must contain at least:

```json
{
  "receipt_id": "unique-id",
  "run_id": "unique-run-id",
  "task_id": "development-or-evaluation-id",
  "arm": "A|B|C",
  "seed": "configured-seed",
  "check_id": "normalized-visible-check-id",
  "command": "allowed-visible-test-command",
  "command_definition_sha256": "hex",
  "workspace_revision_at_start": 0,
  "workspace_revision_at_end": 0,
  "workspace_sha256_at_start": "hex",
  "workspace_sha256_at_end": "hex",
  "environment_sha256": "hex",
  "timestamp_start": "ISO-8601",
  "timestamp_end": "ISO-8601",
  "exit_code": 0,
  "tests_collected": 1,
  "tests_passed": 1,
  "tests_failed": 0,
  "tests_errored": 0,
  "stdout_sha256": "hex",
  "stderr_sha256": "hex",
  "provenance": "agent|controller_recheck|final_gate|common_initial",
  "status": "CURRENT_PASS|CURRENT_FAIL|STALE|ERROR|NOT_EXECUTED"
}
```

Schema above is the **planned contract**, not a claim that the existing notebook already produces these fields. In implementation, normalise types and optional fields and version the JSON schema.

**Workspace identity:** Compute the fingerprint from bytes of a frozen inclusion set covering tracked source, relevant test and configuration files, and eligible untracked source/configuration files. Record additions and deletions. Exclude transient caches, sandbox output and measurement artifacts using an explicitly frozen exclusion list. Record a separate fingerprint for dependency/environment settings. **Git HEAD alone is insufficient** because Gemma edits uncommitted files. A monotonic revision advances only when relevant bytes actually change. Shell-mediated changes must be detected, not only `edit_file` / `write_file`.

**Conservative validity rule:** Any relevant workspace revision invalidates earlier passing receipts for the selected check set. Reverting source to identical bytes does **not** revive an earlier receipt under the primary revision-based protocol. Selective dependency-aware invalidation is out of scope for the main study.

## 4. Frozen experimental conditions

| Arm | What agent can do | Controller behaviour | Independent grading |
|---|---|---|---|
| **A — ordinary repair** | Same v2.7 repair tools, autonomous tool selection | Observe using a **shadow ledger only**; no additional visible-test requirement or feedback | Fresh protected grading of candidate patch |
| **B — final-state gate** | Same v2.7 tools and autonomy | At submission, require successful defined visible validation on **final** revision; run/re-run it when missing or stale, within the same budget; reject unverified acceptance | Same independent protected grading |
| **C — continuous freshness** | Same v2.7 tools and autonomy | **Exactly B's final gate** plus tracking and bounded **automatic reruns after relevant edits** of previously passing selected visible checks; return concise stale/current feedback before next decision | Same independent protected grading |

**Design guardrails:**

- **No mandatory investigation tools.** Gemma does not have to invoke `read_file`, graph tools or any particular diagnostic sequence. B and C enforce the **validity of evidence**, not a tool-use ritual.
- Condition B and C use **identical final-gate implementation**. C's only added intervention is continuous revision tracking, proactive rechecks and provenance-aware feedback during repair.
- All arms have the same model and weights, runtime, v2.7 baseline agent scaffold, model token/context settings, available coding tools, retrieval limits, protected tests and execution budget. Arm-specific additions must be explicitly recorded.
- All arms use the **same passive instrumentation** to count edits/test receipts; A and B do not receive C-specific intermediate freshness messages.
- Controller-triggered rechecks in C, and final-gate tests in B/C, consume the same shared wall-time/tool/test budget. No free extra calls, reset clocks, or privileged hidden tests.
- A fresh visible-test pass is **not** proof of task completion; only independent protected verification determines the repair endpoint.

### Condition C event rule

1. Capture workspace fingerprint before and after each action capable of modifying relevant files.
2. If revision changes, invalidate previously passing **selected** visible-check receipts; do not invent a recheck on the first edit when no selected pass exists.
3. Re-execute the previously passing selected checks within remaining shared budget. Capture a new receipt for the current revision.
4. Send only a compact truthful status to Gemma: which checks became stale, which were rechecked, their current result and whether budget prevented rechecking.
5. Never relabel old failure as pass; never expose protected evaluator output or reference fixes.
6. Apply B's final gate to C's final candidate exactly as in B.

If remaining budget is insufficient, record **UNVERIFIED_BUDGET** rather than treating stale evidence as a pass.

## 5. Hypotheses and outcomes

**H1 — Exposure (descriptive):** In ordinary Gemma repair, there exist trajectories in which a visible test passes and a later relevant code edit occurs without immediate matching revalidation. Measure qualifying pass→edit events and their prevalence, not just raw edit count.

**H2 — Primary quality hypothesis:** Under matched tasks and budgets, **Condition C has higher independently protected-verified repair success than B**. Primary null: no difference. Retain negative or zero effects.

**H3 — Earlier detection:** C may expose **observed** post-pass regressions sooner than B when there are discriminating visible tests. Measure from the **first relevant edit invalidating the previous qualifying pass** to the first valid failed execution of that same check under the same definition and environment. Report non-detection and untested cases separately. C may detect more failures simply because it executes more tests; interpret timing as exploratory unless replay supplies comparable exposure. Merely setting STALE is not a detected regression.

**H4 — Cost trade-off:** Continuous checks change test-execution count, tool/model calls, token use and elapsed time. Report gains and costs together. C may lose correct repairs because rechecks exhaust its budget.

### Outcome hierarchy (do not conflate)

1. **Agent claim:** Gemma says it fixed the task.
2. **Controller acceptance:** B/C gate allows the candidate. Report false rejects and no-decision separately.
3. **Protected verification:** Fresh evaluator succeeds or fails after applying only the final production patch to a clean source snapshot. **Primary quality endpoint.**
4. **Exposure:** Passing evidence becomes stale due to later relevant revision change.
5. **Observed regression:** A new actual test fails after earlier pass.
6. **Resources:** elapsed time, model calls, tool calls, tokens, test count, test time, truncation and timeout.

For comparisons, use the assigned qualified task as the independent unit. Keep paired seeds together within task/repository/commit clusters. Report C−B absolute success-rate difference, uncertainty and the eligible denominator; do not treat tool calls or receipts as independent samples. Preserve task eligibility independently of treatment success.

## 6. Pre-evaluation protections

- Keep the frozen **36 evaluation task IDs** inaccessible to implementation tuning. Do not run the model, inspect protected verification patches or optimise to evaluation failures.
- Development validation only on the six GP02B tasks and **deterministic CPU fixtures**.
- Freeze selected visible-check manifests using only permitted issue/repository information, not hidden test or reference-patch content.
- Preserve original v2.7 results; implement B/C in separately versioned files and runs.
- Freeze code and package hashes, model/template, controller settings, task manifest, shared budget-accounting, seeds, exclusion rules, metrics and the report generator before first confirmatory evaluation.
- Follow the same safe tool protections in A/B/C; **no** mandatory `read_file` or `get_code_neighbors` prerequisite may reappear.
- Clearly label automatic-recheck tokens/messages as a treatment component; C's difference from B is an intervention, not a free engineering improvement.

## 7. GP02C-02 CPU-only acceptance fixtures (revised acceptance gate)

Implement deterministic fixtures before any Gemma run. Minimum contract checks:

| Fixture | Expected outcome |
|---|---|
| PASS at r1 → source edit r2 | r1 marked STALE; no automatic regression claim |
| PASS r1 → edit r2 → rerun PASS | new r2 CURRENT_PASS |
| PASS r1 → edit r2 → rerun FAIL | r2 CURRENT_FAIL and observed regression |
| Edit attempt rejected; bytes unchanged | revision unchanged, no false invalidation |
| `run_command` modifies a source file | revision increases; receipts invalidated |
| Test mutates source while executing | receipt not qualified as CURRENT_PASS |
| Test returns exit 0 but collects zero tests | not a valid pass |
| Test errors, times out or lacks parsable results | ERROR / UNVERIFIED; no accepted pass |
| Environment/dependency fingerprint changes | earlier receipts stale or unqualified |
| Source later reverts to old identical bytes | prior receipt remains stale under revision rule |
| No prior selected passing receipt → edit | no fabricated automatic recheck |
| C rerun budget exhausted | UNVERIFIED_BUDGET; no free time or calls |
| B final-state edit after prior PASS | final gate rechecks current source; cannot accept stale PASS |
| Protected test/reference content supplied to model feedback | explicitly rejected / never exposed |
| A/B/C attempt editing without `read_file`/graph first | not blocked solely for lacking mandatory investigation calls |

**Acceptance for GP02C-01:** Defined constructs, intervention separation, hypotheses, budget and outcome semantics, and deterministic implementation probes. **GP02C-02 v1.2** adds strict visible manifest, capture-and-grade attempt protocol, final patch binding, bounded recheck caps and budget reserve; v1.1 added 16 tests (46 total), and v1.2 adds ten more (56/56 passed locally). This remains a CPU-only contract test, not 36-task evaluation.

## 8. Open decisions before design freeze

1. **Visible-check manifest:** per-task selected checks and exact command normalization, chosen only from allowed sources.
2. **Controller wrapper:** hook points to observe all workspace-changing actions and test completion without modifying the nine agent tool schemas.
3. **Common budget implementation:** verify rechecks and final gate consume the same 15-minute / 60-model-call / 60-tool-call ceilings as autonomous actions; if not, correct across B/C and document.
4. **C feedback:** fixed, compact schema and response token ceiling; no unbounded extra context.
5. **Repetition:** one fully paired seed or additional paired seeds depending on measured compute; decide before touching evaluation tasks.

**Decision at this checkpoint:** GP02C-01 provides the scientific and implementation contract. It does not claim novelty, increased pass rate or successful controller implementation. Proceed to GP02C-02 CPU-only ledger/fixtures once this contract is accepted.


## 9. GP02C-01 v1.2 amendments — mandatory experimental acceptance rules

This section is normative. It resolves reviewer items 1–7 before live development integration. Earlier sections remain the design foundation; where a detail differs, the more explicit v1.2 rule controls.

### 9.1 Final-state capture, independent grading and delivery are distinct (acceptance item 1)

For **every assigned (task, arm, seed)**, capture the final *production-code-only* patch from the last stable workspace when the worker terminates, regardless of whether it called `submit_patch`, exceeded its budget, or was rejected by B/C. Preserve scratch/test artifacts separately but exclude them from the candidate production patch under one **frozen, identical A/B/C extraction rule**. If the patch is empty, corrupt, cannot be applied or the evaluator cannot run, record the appropriate `NOT_RUN`/`INFRA_ERROR`/`FAIL` state rather than hiding it. A genuine agent timeout with no captured production patch is a **repair failure**, with protected grading `NOT_RUN`; infrastructure faults retain `INFRA_ERROR` as a distinct termination/fault classification even when no protected test ran. An independently gradeable patch from a timed-out or rejected attempt can still receive protected PASS, but cannot count as verified delivery.

Where a patch can be applied, the **protected verifier** grades it independently in a fresh evaluator-only snapshot with no agent access to tests or diagnostics. Protected verification is reported as PASS / FAIL / NOT_RUN / INFRA_ERROR. It does **not** depend on `submit_patch` or controller acceptance. **Verified delivery** additionally requires an explicit submission, arm-specific controller acceptance (A has no final gate), an unmodified patch corresponding exactly to the approved snapshot, and protected PASS. Preserve agent claim, submitted state, gate outcome, protected grade and delivered state separately.

The original v2.7 aggregate **3/6 PATCH_VERIFIED** should be labelled as the runner's **submission-conditioned delivered outcome**. A separate protected-grade-for-all-final-snapshots retrospective audit may produce a different descriptive count; do not silently relabel 3/6 as that new count.

### 9.2 Fully specified visible-check receipts (acceptance item 2)

Freeze the visible-check manifest *before* running the assigned evaluation task, based only on permissible agent-visible repository material: unique check ID, exact argv tokens (not just a shell string), relative working directory, test selection, relevant environment fingerprint including installed source/wheels and dependency manifest, check-definition fingerprint, and expected collected test count. Use a typed trusted adapter, not a model-reported result. Include skips in the observation and require `passed + failed + errored + skipped == collected`. All-skipped results, zero collection, contradictory exit codes, wrong collection, malformed JUnit, timeouts and setup errors **must never qualify as CURRENT_PASS**.

Raw receipts are immutable; freshness is recomputed as a separate current-status view. Source fingerprints at test start and end match only at those two instants; they are not proof that no intermediate source mutation occurred. GP02C-03 should execute selected checks against an isolated source snapshot or an equivalent locked no-mutation execution environment. Log all wall-clock intervals and hashes; do not claim isolated-snapshot assurance from the CPU simulator.

### 9.3 Deterministic C schedule and identical B/C gate (acceptance item 3)

- Observe all state-changing agent-tool boundaries (including shell commands), and maintain a shadow ledger in A, B and C.
- C automatically rechecks **previously CURRENT_PASS selected checks** after a relevant mutation, in frozen manifest order, at most **4 selected rechecks per edit** and **12 proactive rechecks per run** (initial development values; reconsider only on development data before final freeze). If no prior passing receipt exists, no automatic recheck is invented. If the test itself mutates code, never approve its result as current.
- Record check caps or insufficient budget as `UNVERIFIED_RECHECK_LIMIT` / `UNVERIFIED_BUDGET`, not as passing evidence. Recheck costs count as shared tool calls and elapsed time. This policy's maximum and feedback shape remain fixed per evaluation run.
- B and C use **one identical** final gate, with up to **3 × manifest-check-count** final check executions to avoid unbounded reruns on mutating tests. At final submission, both can **reuse** an already qualifying CURRENT_PASS receipt on the *exact final code revision*; otherwise both run the missing/stale selected tests. A later fail/error supersedes a pass and blocks acceptance until a new qualifying run resolves the problem. No extra final execution is required merely because the existing PASS was produced before submission on the same unchanged revision.
- Reserve **60 seconds of the common 900-second budget** for B/C final validation. C's automatic rechecks must not consume that reserve. For the live adapter, use a monotonic absolute **840-second proactive/ordinary work deadline** and **900-second overall deadline**, cancel real subprocesses at the limit, and mark incomplete checks `UNVERIFIED_BUDGET`. Synthetic time estimates and post-hoc elapsed accounting do not enforce a hard cutoff; GP02C-02 v1.2 treats overdrafts as unqualified and leaves real cancellation to GP02C-03. The reserve and all controller executions count against the same 900 seconds and 60 tool/model call caps. If tests cannot complete, mark unverified. A follows ordinary baseline behaviour without this intervention reserve; primary comparison remains matched B versus C.
- Capture the complete accepted source workspace as an **immutable snapshot identity**, with trusted production-patch SHA-256, full relevant-workspace SHA-256, revision and frozen manifest identity. Recompute selected receipt statuses after any final synchronization, including any mutation seen between status calculation and export. At delivery recapture and compare **both** workspace SHA and production-patch SHA before declaring verified delivery. A last permitted successful recheck must be evaluated before `UNVERIFIED_RECHECK_LIMIT`. Record a **trusted production-patch SHA-256** and workspace fingerprint/revision when the final gate accepts. If the final patch changes after the gate or while validation is running, delivery requires a new matching gate. Do not accept an earlier receipt for the later patch.

### 9.4 H3 detection timestamp and exposure analysis (review item 4)

For each qualifying pass P at r1, record the first edit event invalidating P at r2 and the completion of the **first valid failure for the same selected check** with unchanged test definition/environment. Time-to-detection is *failure-completion timestamp minus first invalidating-edit timestamp*. Report tests that were not rerun separately; C executes more tests by design, so unequal detection coverage confounds simple latency differences. Supplement with development-only controlled replay where feasible. Do not interpret `STALE` as a fail.

### 9.5 Attempt selection and statistical freeze (review item 5)

Every attempt is immutable and has attempt index, task/arm/seed, code/env fingerprints, termination reason, final patch, explicit submission, gate decision and separately protected grading. The **first** attempt is the scored one; replacement is allowed solely for independently documented infrastructure failure under an identical frozen configuration. Preserve every attempt's `termination`, `patch_present`, `protected_result`, `verified_delivery`, final workspace identity, environment fingerprint and configuration fingerprint. An infrastructure retry requires a supporting evidence ID and matching environment/configuration fingerprints; a no-patch infrastructure error must not disappear into an unqualified `NOT_RUN` label. Preserve the original row and justify every retry. Do not rerun failed model repairs to replace their results. Freeze task qualification rules, single-seed paired task-level **C−B** difference in protected PASS proportion, confidence interval method (paired task or repository-group clustered bootstrap, predeclared cluster scheme), handling of incomplete pairs, and all secondary metrics before evaluation. Missing model outcomes and timeouts count as failures for assigned tasks; genuine verifier infrastructure faults receive `INFRA_ERROR` and the predeclared restart protocol.

Development candidates and held-out evaluation candidates were reported non-overlapping by ID and repository/base-commit group; independently checksum and recheck the frozen manifests before GP03. Disclose that `rich_4079` entered the six-case development set after earlier qualification/repair outcomes were seen. This is **development-set selection provenance**, not independent confirmatory evaluation evidence.

### 9.6 GPU-quota and feasibility gate (review item 6)

With a **15-minute maximum per attempt**, 36 × 3 = 108 A/B/C agent runs × 15 minutes = **27 execution hours**, approximately **54 quota hours** on L4×4 at the officially published 2× debit rate, excluding model startup, task setup, and independent verification. B/C only: 36 × 2 × 15 minutes = **18 execution hours**, approximately **36 quota hours**. These are *upper bounds*, not measured forecasts or entitlements. Use actual GP02B duration distributions and Kaggle's live remaining quota to freeze a feasible cohort and fully paired design **before** exposure to held-out evaluation outcomes. A larger underfunded study is not preferable to a complete, predeclared, reproducible comparison.

### 9.7 Novelty boundary and natural-exposure gate (review item 7)

**Prior work:** Gao, Yang & Yang (2026), *Looping Is Not Reliability: State-Bound Evidence and Typed Revision Contracts for Agentic Code Repair*, [arXiv:2607.24604](https://arxiv.org/abs/2607.24604), already addresses state-bound verifier evidence and reliable admission of code revisions. The paper must cite and compare the actual intervention, not claim originality for hash-bound receipts, test reruns or verification gates. Our target comparison remains *proactive rechecks and feedback during repair versus the identical final gate at fixed budget*. That comparison estimates the **combined effect** of extra test execution and intermediate feedback, not the separate causal contribution of the ledger alone.

**Critical feasibility condition:** before spending L4 quota on three-arm main experiments, audit the six development trajectories for authentic **passing visible selected test → subsequent relevant source edit** intervals, with command success, test counts and revision identity. An initial tool-event screening of explicit pytest commands showed **no confirmed PASS→later production edit** in these six traces; this is a preliminary *command-level* screen, not a verified full receipt audit. The pattern may differ under B/C, but without enough naturally occurring exposures Condition C could have little opportunity to act. If exposure is rare, report the boundary and use separately labelled controlled development replay/stress tests; do not contaminate the 36 evaluation cohort, manufacture natural effects or quietly change task selection based on evaluation results.

### 9.8 Implementation acceptance status and blockers

- CPU **v1.2 prototype: 56/56 deterministic unit tests passed** locally. This extends v1.1's 46/46 and v1.0's 30/30; it validates only synthetic controller contracts.
- **Still needed for GP02C-03:** live trusted execution of real pytest/JUnit; agent-tool integration with no double-billing; isolated immutable test snapshots; realistic hard deadlines and final reserve; production-only patch capture and checksum; protected verifier under process isolation; pass→edit exposure audit; complete paired compute envelope and manifest freeze.
- No change to GP02B v2.7 or any of the six original evidence ZIPs. No held-out task opened or run during CPU fixture changes.

**Bottom line:** Items 1–3 are design/CPU acceptance requirements; the live implementation must independently satisfy them. Items 4–7 are mandatory *protocol and feasibility* decisions before the first confirmatory evaluation. Only B/C's paired held-out results can support the central empirical hypothesis.

---

**Internal basis:** `GP01_Research_Selection_and_Protocol_v1_0.md` (5 Nov 2026); `Gemma4_Evidence_Freshness_Research_and_Implementation_Plan_v1_0.docx` (6 Nov 2026); GP02B v2.7 six-task evidence and decisions discussed on 7 Nov 2026. Adaptations in this GP02C-01 document reflect the completed v2.7 development run and the user-approved removal of mandatory investigation-tool calls.

## 10. GP02C-02 v1.2 corrective release and validation boundary

Controller review issues RV-01/02/04 are addressed in versioned CPU code: the B/C gate rechecks receipt applicability after final synchronization and records full workspace plus production-patch identity; it reviews a passing last-permitted recheck before rejecting; independent attempt assessment preserves infrastructure-fault classification, full delivery identity and independently protected grading. All infrastructure retries require a recorded evidence identifier and unchanged environment/configuration fingerprints.

**RV-03 is only partially addressed at CPU level:** a simulated synchronous executor can run longer than its estimated slot, so v1.2 **records actual elapsed time**, disqualifies a result that breached the allowed budget, and never treats it as verified. It does **not** prevent the elapsed time from being spent. Enforcing an 840/900-second physical time boundary requires GP02C-03's trusted, isolated, cancellable subprocess adapter. Do not claim otherwise.

The v1.2 notebook demonstrates controller rules only. Live agent integration, the real-pytest smoke test, full natural-exposure audit, and held-out evaluation remain unexecuted by this release.
