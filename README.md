# Revision-Aware Repair Verification for Gemma 4 Software Repair Agents

Public research companion for the Gemma 4 Developer Agent Paper Track by Clifford Imaguezegie.

The controller binds a trusted pytest result to the source revision, registered check and environment that produced it. Policy B requires current evidence at submission. Policy C also reruns the check after an observed edit invalidates a trusted pass. Independent protected grading scores the captured patch. Verified delivery additionally requires an accepted submission of that same patch.

## Reviewer entry points

| Resource | What to review |
|---|---|
| [Single reviewer notebook](notebooks/Gemma4_RARV_Kaggle_Companion_v1_5.ipynb) | CPU replay and reconciliation, study notebook links and a downloadable source archive |
| [Evidence index](docs/evidence-index.md) | Notebook stages, accepted runs, archive identities and Appendix B cross-references |
| [E17 CPU package](evidence/Gemma4_RARV_CPU_Review_Package_v1_0.zip) | Frozen controller, synthetic fixture, pinned dependencies, selected sealed JSON and saved validation |
| [Reproduction instructions](evidence/REPRODUCE.md) | Run the checks on Linux with Python 3.12 and Git |
| [References](docs/references.md) | Research literature and official Kaggle material |

## Evaluation scope

| Cohort | Task pairs | Agent runs | Protected passes per policy |
|---|---:|---:|---:|
| GP03 primary autonomous | 9 | 18 | 6/9 |
| GP04 and GP05 autonomous follow-up | 3 | 6 | 1/3 |
| GP04 controlled activation | 2 | 4 | Reported separately as detection and recovery evidence |

All nine primary pairs resolved. Verified delivery was 5/9 for B and 4/9 for C. The primary paired difference was 0.0 percentage points with a 95% repository-stratified bootstrap interval from −22.2 to +22.2 points. One rollout per policy per task limits precision. Across 24 autonomous runs, no trusted pass was followed by a qualifying edit. The two controlled C detections occurred in 1.150 and 1.047 seconds before inference.

## Evidence and materials

The public notebooks retain study-authored source and provide links to the organiser inputs. Historical notebook outputs, task data, protected checks, reference patches and candidate diffs remain in their authorised locations. The frozen review package includes its wheel licence notices. Saved protected verdicts support reconstruction of the recorded results. Fresh protected grading uses the organiser's evaluator and inputs.
