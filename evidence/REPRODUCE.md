# Reproducing RARV: CPU review, original GPU study and sealed-archive audit

This guide separates three materially different reproduction claims. The primary reviewer entry point is the [public Kaggle companion](https://www.kaggle.com/code/cliffordimaguezegie/gemma4-revision-aware-repair-verification/notebook), whose preserved GitHub source is [`Gemma4_RARV_Kaggle_Companion_v2_6.ipynb`](../notebooks/Gemma4_RARV_Kaggle_Companion_v2_6.ipynb). The notebook identifies itself as v2.6; the Kaggle hosted run's Version 3 is a Kaggle-version identifier, not a claim that the code is v3.

| Route | Evidence claim | Model calls | Original input needs |
|---|---|---|---|
| **A. E17 CPU reviewer checks** | Re-executes the frozen controller on synthetic source; reconstructs recorded results from selected archived JSON and pins | **0** | E17 public dataset or the bundled review ZIP |
| **B. GP00–GP05 GPU study procedures** | Re-executes original methodology where stage inputs are available; new runs may differ from recorded outputs | **Required in live model stages** | Competition task/repo inputs, pinned model/runtime, compatible GPU, and relevant fixtures; protected grading needs authorised assets |
| **Optional. Seven-archive candidate audit** | Checks complete sealed ZIP hashes and all **28** recorded candidate records and their identities | **0** | Seven original sealed archives; not distributed with public evidence |

## Path A — public Kaggle notebook or local E17 CPU verification

The notebook provides a reviewer map, vector architecture diagrams, E17 reproduction, and optional inspection of the *actual frozen Python modules*. In Kaggle, attach [E17 public data](https://www.kaggle.com/datasets/cliffordimaguezegie/gemma4-rarv-evidence), open the public notebook and execute the three labelled code cells in order. The first draws diagrams; the second runs CPU verification; the third optionally maps code to architecture. The notebook reports a separate successful Kaggle Version 3 CPU run on Python 3.13.18, but its **embedded saved outputs derive from prior local Python 3.13.5 validation**. Run all cells in Kaggle to generate hosted outputs for this exact notebook; archived outputs alone do not constitute a new execution.

For an independent local reproduction, download [`Gemma4_RARV_CPU_Review_Package_v1_0.zip`](Gemma4_RARV_CPU_Review_Package_v1_0.zip), extract it and use the commands below. The **E17 ZIP v1.0** originally accompanied capstone v1.7; retaining the frozen version preserves reproducibility for the later v1.8 report. The local reference procedure uses **Linux, Python 3.12 and Git**, even though the published Kaggle notebook separately records its hosted Python 3.13 run.

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

## Path B — original GP00–GP05 study procedures (GPU and original data)

These are **not** the E17 CPU fixture. They operate on real benchmark repositories, registered checks, agent tools and protected evaluation where available. See the [evidence index](../docs/evidence-index.md) for the stage-by-stage notebooks. The published implementation describes the original methods but does not bundle every competition asset or sealed historical run.

| Study stage | Main purpose | Additional requirements |
|---|---|---|
| GP00–GP01 | Task inventory, harness and reference qualification | Public task/repository snapshots; original evaluator inputs where the notebook invokes them |
| GP02B / GP02C | Development studies, source identity, ledger, trusted pytest, live agent attachment | Stage-specific fixtures and offline dependencies; compatible GPU + pinned checkpoint for live Gemma calls; CPU-specific cells do not require inference |
| GP03 | Qualified matched B/C autonomous runs, independent grading and accepted-patch reconciliation | Frozen harness and task inputs, relevant test/fixture wheels, model + GPU for live runs, authorised protected evaluator for fresh independent grading |
| GP04 / GP05 | Controlled activation and supplemental autonomous follow-up | Original task snapshots, scripted injections where applicable, pinned environment and authorised grading inputs for fresh reruns |

**Recorded live configuration:** Gemma 4 31B IT QAT W4A16 checkpoint v2, vLLM 0.19.1, four NVIDIA L4 GPUs, pinned harness dependencies and the study's saved seeds/tool budgets. Exact stage notebook configurations remain authoritative. An independently rerun stochastic agent may produce a different trace or patch despite matching settings.

### Kaggle input mounts documented for the original study

| Asset | Input mount |
|---|---|
| Competition tasks and repository snapshots | `/kaggle/input/competitions/gemma-4-developer-agent` |
| Datafix supplement | `/kaggle/input/datasets/cliffordimaguezegie/datafix` |
| Requests fixture wheels | `/kaggle/input/datasets/cliffordimaguezegie/gp03-requests-fixture-wheels-v1` |
| GP03 test wheels | `/kaggle/input/datasets/cliffordimaguezegie/gp03-test-wheels-v2` |
| Organiser wheelhouse | `/kaggle/input/datasets/metric/gemma-4-developer-agent-wheelhouse` |
| Gemma 4 QAT model checkpoint v2 | `/kaggle/input/models/google/gemma-4/other/gemma-4-31b-it-qat-w4a16-ct/2` |
| E17 CPU public evidence | `/kaggle/input/datasets/cliffordimaguezegie/gemma4-rarv-evidence` |

These mounts were recorded for this project. The notebooks' individual requirements are stage dependent; attaching every input to every notebook is not necessarily required. **The E17 CPU companion requires only its E17 evidence input**, not the competition dataset or the Gemma model.

### Scope of the independent evaluation claim

Protected passes refer to independently graded captured production patches, including deadline captures that may not have been delivered. **Verified delivery** additionally requires an accepted submission for the identical patch. GP03 yielded 6/9 protected passes per arm, with verified deliveries B 5/9 and C 4/9; the proactive mechanism had no qualifying natural pass-to-edit triggers in the 24 autonomous runs. Controlled fault detection did not establish higher autonomous repair success. The published reviewer CPU checks reconstruct recorded outcomes but do **not** rerun protected tests.

## Optional — full historical sealed-archive audit

Use the already documented `reconcile_results.py --archives /path/to/archive_directory` command in Path A's instructions above. This option adds verification of the seven complete original archive ZIP digests, their member manifests, candidate diffs, ledger/gate identities and all 28 accepted captured candidate records. **The public GitHub archive and Kaggle E17 reviewer dataset do not include those seven sealed ZIPs**; reviewers without them should use bundled reconciliation and state its narrower scope accurately.

## File integrity

From the repository root, use `sha256sum -c SHA256SUMS` on Linux to check files present in the curated manifest. The notebook's GitHub source is byte-identical to the user-supplied v2.6 `.ipynb`, including the outputs inherited from local Python 3.13.5 validation. The E17 ZIP remains pinned by its unchanged SHA-256 `3b8c15a8433909c09fab11cce28f136c8ef9d195e2769f237c24b71315cd96c0`.
