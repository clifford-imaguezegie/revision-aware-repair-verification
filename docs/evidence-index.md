# Evidence Index

This index follows Appendix B of the capstone. Links lead to study source copies, derived evidence and reproduction tools. Original run archives are identified by filename and digest for authorised verification.

## E01

**Task inventory and split.** [GP00 task audit](../notebooks/GP00_Task_Audit.ipynb) covers the 129 published records, the six initial development choices and the 36 provisional evaluation candidates.

## E02

**Evaluator qualification.** [GP01A harness check](../notebooks/GP01A_Harness_Check.ipynb) · [GP01B reference qualification](../notebooks/GP01B_Reference_Qualification.ipynb) · [GP01C retry](../notebooks/GP01C_Qualification_Retry.ipynb). These notebooks use participant access to the official evaluator inputs.

## E03

**Study protocol.** [GP02C contract and hypotheses v1.2](GP02C_Protocol_v1_2.md) records the planned receipt contract, policies and endpoints. Sections 3 and 6 of the capstone report the final evaluation and amendments.

## E04

**Development and feasibility.** [Six development cases](../notebooks/GP02B_Six_Development_Cases.ipynb) · [Revision ledger](../notebooks/GP02C_Revision_Ledger.ipynb) · [Trusted pytest adapter](../notebooks/GP02C_Trusted_Pytest_Adapter.ipynb) · [Live attachment](../notebooks/GP02C_Live_Attachment.ipynb).

### GP02C live pilot

The Rich development pilot exercised both policies on the same task. The controller recorded no qualifying autonomous pass-to-edit exposure.

### GP02C scripted replay

The original Rich scripted replay recorded B as stale and C as current failure after a deliberate change. Both gates blocked the changed state and accepted the restored state. The later [E17 CPU check](#e17) executes the frozen GP03 fixture.

### GP02C guided case

The guided FastAPI candidate passed three registered checks and seven protected tests after the independent CPU evaluator applied the captured patch. This guided development evidence is separate from the held-out analysis.

## E05

**Evaluation readiness.** [CPU qualification and retry](../notebooks/GP03_CPU_Qualification.ipynb) · [Public adapter acceptance](../notebooks/GP03_Public_Adapter.ipynb) · [Paired CPU acceptance](../notebooks/GP03_Paired_CPU_Acceptance.ipynb) · [Official worker acceptance](../notebooks/GP03_Official_Worker_Acceptance.ipynb). Public checks ran on the selected nine tasks. Their baseline passes establish runnable checks for existing behaviour.

## E06

**GP03 freeze and accepted execution.** [Frozen evaluation notebook](../notebooks/GP03_Frozen_Evaluation.ipynb) documents the accepted v1.1 transport. The freeze SHA-256 is `28b958752eaca3034eaed5d05ce1069f0f356681ca7c731a825e93a44e5268d4`. The two accepted wave records follow.

### GP03 wave 1

`GP03_Evaluation_28b958752eac_Wave1_GP03_Session_20261109T114916Z_4fe2b9bf.zip`  
SHA-256 `7b9d91ade35ac6fa9d134cc515ccd800ce6e36b6ccc3ef5118880910d8bf8432`.

### GP03 wave 2

`GP03_Evaluation_28b958752eac_Wave2_GP03_Session_20261109T131009Z_26b3be8f.zip`  
SHA-256 `71700ac891c04ae3d38ccce2d2ea4fafff424c261cda077e2d419e98234bb814`.

## E07

**Interim CPU grading.** [Independent grading notebook](../notebooks/GP03_Independent_Grading.ipynb). The first grading pass resolved six of nine pairs. [E11](#e11) is the source for the final primary result.

## E08

**GP04 preflight.** [GP04 evidence notebook](../notebooks/GP04_Followup_Evidence.ipynb). Two additional autonomous pairs qualified on public readiness and protected baseline/reference checks. The two controlled pairs qualified on the known-good starting state and a deliberately introduced test failure. Sealed preflight `GP04_CPU_Preflight_20261109T184534Z_fdce7547.zip`.

## E09

**GP04 GPU execution.** `GP04_GPU_Execution_20261109T203725Z_81f17a41.zip`  
SHA-256 `983bc908991f06c31ff315ea795ae6dc861175891c1879f1f00dad13efe44f3b`. The trace summary records controlled detection at 1.150 and 1.047 seconds.

## E10

**GP04 independent CPU grades.** `GP04_CPU_Grades_20261109T221506Z_f38ba18a.zip`  
SHA-256 `1c00b32c0c62a8fafb5f260382371a347480276de29859c5ec3c5ede78812b98`. Each policy had one protected pass across the two additional autonomous tasks.

## E11

**Final GP03 recovery and grades.** [CPU recovery notebook](../notebooks/GP03_Final_Grade_Recovery.ipynb). Final record `GP03_Targeted_CPU_Grades_20261110T110823Z_9641559d.zip`  
SHA-256 `6d0b6fbfb3c01818ebdd116a389b66758e727cc77851b808a38e15f79751f294`. All nine pairs resolved. Protected passes were 6/9 for each policy. Verified delivery was 5/9 for B and 4/9 for C.

### GP03 FastAPI recovery

The intermediate `GP03_Targeted_CPU_Grades_20261110T101401Z_5cd919f1.zip` resolved the two FastAPI pairs. The final record above also resolves the Requests pair.

## E12

**GP05 preflight.** [Rank 12 notebook](../notebooks/GP05_Supplemental_Pair.ipynb). The supplemental Rich task passed the public check and reproduced the protected baseline failure that the reference repaired. Sealed preflight `GP05_CPU_Preflight_20261110T114208Z_c917fe81.zip`.

## E13

**GP05 GPU execution.** `GP05_GPU_Execution_20261110T114809Z_2a53ab37.zip`  
SHA-256 `d6f2661af96415237e5e91fff97bcc99bc5a5c0524964405338641b1052380ba`. Both attempts reached the model-call cap.

## E14

**GP05 independent CPU grades.** `GP05_CPU_Grades_20261110T122452Z_3b5ea674.zip`  
SHA-256 `5f41a57ea546401811fbd9ebb8f883989179f4b5c29b7fc6a52b68217ce863ba`. B had an empty capture and C passed 97 of 98 protected tests. Both scored zero.

## E15

**Coding improvement register.** The internal development register groups observations about issue understanding, repair verification, budgets, tools, context and patch quality. Appendix C of the full capstone describes the follow-up workstreams.

## E16

**Planning input.** The internal planning record informed future engineering priorities. The study reports its evaluation results separately from that planning material.

## E17

**Executable CPU review package.** [Download Gemma4_RARV_CPU_Review_Package_v1_0.zip](../evidence/Gemma4_RARV_CPU_Review_Package_v1_0.zip)  
SHA-256 `3b8c15a8433909c09fab11cce28f136c8ef9d195e2769f237c24b71315cd96c0`.

[Run instructions](../evidence/REPRODUCE.md) · [Archive provenance](../evidence/Archive_Provenance.json) · [CPU replay validation](../evidence/validation/Replay_Report.json) · [Bundled result reconstruction](../evidence/validation/Results_Bundled.json) · [Full archive validation](../evidence/validation/Results_Original_Archives.json).

The package runs the frozen controller on a synthetic workspace and reconstructs results from selected sealed JSON records. Archive mode additionally verifies candidate bytes when the seven original ZIPs are available. Fresh protected grading requires the organiser evaluator and its protected inputs.
