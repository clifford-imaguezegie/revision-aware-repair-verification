# Revision Aware Repair Verification CPU Review Package

Version 1.0 accompanies the full Gemma 4 capstone v1.7. The package contains the frozen GP03 controller and scoring modules, the scripted acceptance fixture, and byte-identical JSON members selected from the study archives. Linux, Python 3.12, and Git are the execution prerequisites. The five bundled wheels supply the pinned pytest environment offline. Both checks make zero model calls.

## Execute the checks

From the extracted package directory, run

    python3.12 -m venv .venv
    .venv/bin/python -m pip --isolated install --no-index --find-links wheels --require-hashes -r requirements.txt
    .venv/bin/python replay_controller.py --output review_replay
    .venv/bin/python reconcile_results.py --output review_results.json

Use a new output path for a repeat execution so the earlier receipts remain available. The results checker also runs with Python 3.12 and its standard library before installing pytest.

## Actual controller replay

The fixture invokes the pinned attach_gp03_pair path with scripted tool requests on a small synthetic Git checkout. The real trusted adapter executes pytest. The replay checks the source identity, verifies that a partial command creates no receipt, and exercises the evidence transitions and submission gate.

| Checkpoint | B final gate | C proactive recheck |
|---|---|---|
| Initial trusted pass | CURRENT_PASS | CURRENT_PASS |
| Scratch file edit | CURRENT_PASS | CURRENT_PASS |
| Production edit | STALE | CURRENT_FAIL |
| Broken state at submission | Blocked | Blocked |
| Restored state at submission | Accepted | Accepted |

Each arm records three trusted checks. B obtains the failure when the gate checks the stale receipt. C obtains it when the edit triggers its recheck. The fixture also tests the ordinary repair cutoff and final reserve. The output includes Replay_Report.json, both ledgers, gate audits, and JUnit receipts. Acceptance after restoration demonstrates the gate transition on the fixture.

## Reconstruct the archived outcomes

The checker verifies the provenance index, the pinned freeze and scorer, and twelve original JSON member digests. It joins accepted attempts to independent protected verdicts by attempt ID and exact candidate SHA-256. Empty production captures score zero under the frozen rule. Verified delivery also requires the accepted gate identity. The checker rebuilds the cohort counts and the 10,000-draw GP03 repository-stratified bootstrap.

If you have the seven original ZIPs listed in evidence/Archive_Provenance.json, run

    .venv/bin/python reconcile_results.py --archives /path/to/archive_directory --output review_original_results.json

This mode verifies the complete ZIP digests and sealed member manifests. It also checks the original per-attempt records, candidate bytes or empty captures, and the saved gate and revision identities against the grades. Both modes reproduce 24 autonomous runs and four controlled runs. The GP03 primary comparison gives six protected passes per arm and five verified deliveries for B and four for C.

The bundled mode verifies the selected original JSON members. The full archive mode adds patch and checkpoint verification. Both reconstruct the recorded outcomes. A fresh protected grading run uses the organizer's evaluator and protected test assets. The CPU fixture tests the controller logic using scripted actions and synthetic source.

## Provenance and saved validation

- evidence/Archive_Provenance.json lists the seven ZIP names and digests, the twelve member pins, and the original fixture notebook and definition hashes.
- frozen/gp03_public_runtime.py and frozen/gp03_eval_score.py match the source pins in the accepted GP03 v1.1 freeze.
- frozen/gp03_cpu_fixture.py preserves the two fixture definitions verbatim from GP03_Paired_Runner_CPU_Acceptance_v1_0.ipynb.
- validation contains the observed replay report and the results from both reconciliation modes.
- The wheels are unmodified packages with their accompanying distribution metadata and licences.
