# Reproducing RARV — CPU review, original GPU study and sealed-archive audit

Start with the [public Kaggle companion](https://www.kaggle.com/code/cliffordimaguezegie/gemma4-revision-aware-repair-verification/notebook) to review the notebook and its saved CPU run. The [saved Kaggle notebook export](../notebooks/Gemma4_RARV_Kaggle_Companion_v2_6.ipynb) contains the notebook source and saved outputs.

| Route | Evidence claim | Model calls | Original input needs |
|---|---|---|---|
| **A. E17 CPU reviewer checks** | Re-executes the frozen controller on synthetic source and reconstructs recorded results from selected archived JSON and checksum pins | **0** | E17 public dataset or the bundled review ZIP |
| **B. GP00–GP05 GPU study procedures** | Re-executes the original methods where stage inputs are available. New runs may differ from recorded outputs. | **Required in live model stages** | Competition tasks and repositories, pinned model and runtime, compatible GPU, and relevant fixtures. Protected grading requires authorised assets. |
| **Optional. Seven-archive candidate audit** | Checks complete sealed ZIP hashes and all **28** recorded candidate records and their identities | **0** | Authorised access to the seven original sealed archives |

## Path A — public Kaggle notebook or local E17 CPU verification

The notebook includes a reviewer map, architecture diagrams, E17 CPU verification, and optional inspection of the frozen Python modules. Attach the [public E17 dataset](https://www.kaggle.com/datasets/cliffordimaguezegie/gemma4-rarv-evidence) to reproduce the CPU checks. Run the three labelled cells in order. The first draws the diagrams. The second runs CPU verification. The third optionally maps the source modules to the architecture. Kaggle’s Logs show the saved hosted CPU run.

For an independent local reproduction, download [`Gemma4_RARV_CPU_Review_Package_v1_0.zip`](Gemma4_RARV_CPU_Review_Package_v1_0.zip). Extract the ZIP before running the commands below.

The **E17 ZIP v1.0** originally accompanied capstone v1.7 and provides the frozen evidence for reproducing the results reported in v1.8. The package contains the frozen GP03 controller and scoring modules, the scripted acceptance fixture, and byte-identical JSON members selected from the study archives. The local procedure uses **Linux, Python 3.12 and Git**. The five bundled wheels supply the pinned pytest environment offline. Both checks make zero model calls.

## Execute the checks

From the extracted package directory, run the following commands.

    python3.12 -m venv .venv
    .venv/bin/python -m pip --isolated install --no-index --find-links wheels --require-hashes -r requirements.txt
    .venv/bin/python replay_controller.py --output review_replay
    .venv/bin/python reconcile_results.py --output review_results.json

Use a new output path for each repeat execution to preserve earlier receipts. The results checker also runs with Python 3.12 and its standard library before installing pytest.

## Actual controller replay

The fixture invokes the pinned `attach_gp03_pair` path with scripted tool requests on a small synthetic Git checkout. The trusted adapter executes pytest. The replay checks the source identity, verifies receipt rejection for partial commands, and exercises the evidence transitions and submission gate.

| Checkpoint | B final gate | C proactive recheck |
|---|---|---|
| Initial trusted pass | `CURRENT_PASS` | `CURRENT_PASS` |
| Scratch file edit | `CURRENT_PASS` | `CURRENT_PASS` |
| Production edit | `STALE` | `CURRENT_FAIL` |
| Broken state at submission | Blocked | Blocked |
| Restored state at submission | Accepted | Accepted |

Each arm records three trusted pytest executions. B obtains the failure when the gate checks the stale receipt. C obtains it when the edit triggers its recheck. The fixture also tests the ordinary repair cutoff and final reserve. The output includes `Replay_Report.json`, both ledgers, gate audits, and JUnit receipts. Acceptance after restoration demonstrates the gate transition on the fixture.

## Reconstruct the archived outcomes

The checker verifies the provenance index, the pinned freeze and scorer, and twelve original JSON member digests. It joins accepted attempts to independent protected verdicts by attempt ID and exact candidate SHA-256. Empty production captures score zero under the frozen rule. Verified delivery also requires the accepted gate identity. The checker rebuilds the cohort counts and the 10,000-draw GP03 repository-stratified bootstrap.

With the seven original ZIPs listed in `evidence/Archive_Provenance.json`, run the following command.

    .venv/bin/python reconcile_results.py --archives /path/to/archive_directory --output review_original_results.json

This mode verifies the complete ZIP digests and sealed member manifests. It also checks the original per-attempt records, candidate bytes or empty captures, and the saved gate and revision identities against the grades. Both reconciliation modes reconstruct recorded outcomes for 24 autonomous runs and four controlled runs. The GP03 primary comparison gives six protected passes per arm, with five verified deliveries for B and four for C.

The bundled mode verifies the selected original JSON members. The full archive mode adds patch and checkpoint verification. Fresh protected grading uses the organiser’s evaluator and protected test assets. The CPU fixture tests the controller logic using scripted actions and synthetic source.

## Provenance and saved validation

- `evidence/Archive_Provenance.json` lists the seven ZIP names and digests, the twelve member pins, and the original fixture notebook and definition hashes.
- `frozen/gp03_public_runtime.py` and `frozen/gp03_eval_score.py` match the source pins in the accepted GP03 v1.1 freeze.
- `frozen/gp03_cpu_fixture.py` preserves the two fixture definitions verbatim from `GP03_Paired_Runner_CPU_Acceptance_v1_0.ipynb`.
- `validation` contains the observed replay report and the results from both reconciliation modes.
- The wheels are unmodified packages with their accompanying distribution metadata and licences.

## Path B — original GP00–GP05 study procedures (GPU and original data)

These procedures use real benchmark repositories, registered checks, agent tools, and protected evaluation where available. Reproducing a stage requires access to its competition assets and any historical archives it uses. See the [evidence index](../docs/evidence-index.md) for the stage-by-stage notebooks.

| Study stage | Main purpose | Additional requirements |
|---|---|---|
| GP00–GP01 | Task inventory, harness and reference qualification | Public task and repository snapshots, plus original evaluator inputs where the notebook invokes them |
| GP02B / GP02C | Development studies, source identity, ledger, trusted pytest, live agent attachment | Stage-specific fixtures and offline dependencies. Live Gemma calls require a compatible GPU and the pinned checkpoint. |
| GP03 | Qualified matched B/C autonomous runs, independent grading and accepted-patch reconciliation | Frozen harness and task inputs, relevant test and fixture wheels, model and GPU for live runs, and authorised evaluator assets for fresh protected grading |
| GP04 / GP05 | Controlled activation and supplemental autonomous follow-up | Original task snapshots, scripted injections where applicable, pinned environment, and authorised grading inputs for fresh reruns |

**Recorded live configuration.** The study used the Gemma 4 31B IT QAT W4A16 checkpoint v2, vLLM 0.19.1, four NVIDIA L4 GPUs, pinned harness dependencies, and the saved seeds and tool budgets. Use each stage notebook for its exact configuration. A fresh agent run may produce a different trace or patch even with matching settings.

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

Attach the inputs required by the stage you are running. **The E17 CPU companion requires only its E17 evidence input.**

### Scope of the independent evaluation claim

Protected passes refer to independently graded captured production patches, including deadline captures. **Verified delivery** additionally requires an accepted submission for the identical patch. GP03 yielded 6/9 protected passes per arm. Verified delivery was 5/9 for B and 4/9 for C. The 24 autonomous runs yielded zero qualifying natural pass-to-edit triggers. The controlled trials demonstrate fault detection after deliberately introduced edits. The reviewer CPU checks reconstruct recorded outcomes from the archive. Fresh protected grading requires authorised evaluator assets.

## Optional — full historical sealed-archive audit

Use the `reconcile_results.py --archives /path/to/archive_directory` command in Path A. This option adds verification of the seven complete archive ZIP digests, their member manifests, candidate diffs, ledger and gate identities, and all 28 accepted captured candidate records. The full audit requires access to the seven original sealed ZIPs. The public GitHub and Kaggle E17 packages support bundled reconciliation of the selected archived records.

## File integrity

From the repository root on Linux, run `sha256sum -c SHA256SUMS` to check the files listed in the manifest. The pinned SHA-256 for the E17 ZIP is `3b8c15a8433909c09fab11cce28f136c8ef9d195e2769f237c24b71315cd96c0`.