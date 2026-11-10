# RARV Notebook Output Restoration — Audit v1.4

Source: author-provided `Paper (2)(2).zip`. Code: prior clear-text publication versions.

## Boundaries

Saved outputs were copied from source notebooks. They are **historical evidence**, not fresh runs. Only text/markdown and safe small HTML were retained; binary download widgets were removed. Three historical runtime configuration cells were restored to keep source and outputs aligned. String-valued source rewrites were AST-tested for equal runtime values. The prior GP02C live attachment source has no saved outputs, and GP04 summary has no executable cells.

Original completed GPU runs require the original competition assets, model, hardware and sealed archives. A reader can inspect preserved results without re-running those sessions. No protected evaluator was run in this restoration.

## Mapping

| Notebook | Original source | Saved outputs | Preserved readable outputs | Notes |
|---|---|---:|---:|---|
| GP00_Task_Audit | `Paper/GP00/dataset-verification.ipynb` | 8 | 8 | with_original_saved_outputs |
| GP01A_Harness_Check | `Paper/GP01/gp01a.ipynb` | 7 | 7 | with_original_saved_outputs |
| GP01B_Reference_Qualification | `Paper/GP01/gp01b (1).ipynb` | 10 | 10 | with_original_saved_outputs |
| GP01C_Qualification_Retry | `Paper/GP01/gp01c.ipynb` | 10 | 10 | with_original_saved_outputs |
| GP02B_Six_Development_Cases | `Paper/GP02/Batch_2/gp02b-v2-7.ipynb` | 148 | 148 | with_original_saved_outputs |
| GP02C_Live_Attachment | `Paper/GP02/GP02C/GP02C_04B_Live_ADK_Gemma_Integration_v1_1.ipynb` | 0 | 0 | Source has no saved output |
| GP02C_Revision_Ledger | `Paper/GP02/GP02C/gp02c-02.ipynb` | 5 | 5 | with_original_saved_outputs |
| GP02C_Trusted_Pytest_Adapter | `Paper/GP02/GP02C/gp02c-03.ipynb` | 7 | 7 | with_original_saved_outputs |
| GP03_CPU_Qualification | `Paper/GP03/gp03-1.ipynb` | 6 | 6 | with_original_saved_outputs |
| GP03_Final_Grade_Recovery | `Paper/GP03/gp03-cpu-recovery-wheels-v15.ipynb` | 5 | 5 | with_original_saved_outputs |
| GP03_Frozen_Evaluation | `Paper/GP03/gp03-6.ipynb` | 6 | 6 | Historical runtime config aligned |
| GP03_Independent_Grading | `Paper/GP03/gp03-8.ipynb` | 6 | 6 | Historical runtime config aligned |
| GP03_Official_Worker_Acceptance | `Paper/GP03/gp03-4.ipynb` | 9 | 9 | with_original_saved_outputs |
| GP03_Paired_CPU_Acceptance | `Paper/GP03/gp03-3.ipynb` | 6 | 6 | with_original_saved_outputs |
| GP03_Public_Adapter | `Paper/GP03/gp03-2.ipynb` | 6 | 6 | with_original_saved_outputs |
| GP05_Supplemental_Pair | `Paper/GP05/gp05-1.ipynb` | 6 | 6 | with_original_saved_outputs |
| GP03_Frozen_Evaluation_Wave2_Executed | `Paper/GP03/gp03-7.ipynb` | 6 | 6 | additional_historical_run |
| GP04_CPU_Preflight_Executed | `Paper/GP03/gp03-10.ipynb` | 6 | 6 | additional_historical_run |
| GP04_GPU_Execution_Executed | `Paper/GP04/gp03-12.ipynb` | 6 | 6 | additional_historical_run |
| GP04_CPU_Grading_Executed | `Paper/GP04/gp03-13.ipynb` | 6 | 6 | additional_historical_run |
| GP04_Followup_Evidence | `documentation only; see three GP04 executed notebooks` | 0 | 0 | documentation_only |

## Publication review

Before publishing saved execution outputs, inspect the full traces for sensitive data, authorised evaluation material, third-party licence restrictions and unpublished competition-protected information. This ZIP excludes entire protected evaluation archives but still contains historical notebook output text.

**Do not present output restoration as a new independent execution or a new Kaggle evaluation.**
