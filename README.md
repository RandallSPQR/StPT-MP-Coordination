# StPT and Missing Piece: shared coordination record

One record for two complementary studies, linked from both repositories:

- **Stigmergic Phase Transitions (StPT)**, revision 3: https://github.com/RandallSPQR/Stigmergic-Phase-Transitions
  (brief: https://claude.ai/artifact/Q36cWtMumtJPs3semD76ni)
- **Missing Piece**, revision 2: https://github.com/RandallSPQR/missing-piece (created 2026-11-03, D32; brief: https://claude.ai/artifact/8KE5pFnmDEdJYGyHTjDcG7)

The two studies are complementary, not matched. They share the scenario format, the evidence schema, the pinned model
pipeline and this record. A matched bridge follows both core studies; its interface lives in `PROTOCOL_SPEC.md`.

| file | holds |
|---|---|
| `DECISIONS.md` | the decision register: accepted, proposed, unresolved |
| `PROTOCOL_SPEC.md` | the shared protocol specification and the evidence-schema version |
| `PIPELINE.md` + `pipeline.yaml` | the pinned pipeline release, with exact model and runtime identities |
| `DATA_SEPARATION.md` | the development / confirmatory data separation record |
| `AMENDMENTS.md` | the chronological log; prospective amendments and post-outcome deviations kept apart |
| `PROVENANCE_INDEX.md` | claim → analysis → results |

**Rules.**
- Every study manifest references the exact commit of this record that it used, plus the pipeline tag and commit.
- Prior versions are preserved: entries are appended or superseded, never edited away.
- A change to the pinned pipeline is a new pipeline version and a logged entry in `AMENDMENTS.md`.
- Numbers are copied only from a results file, and that file is cited.

Status (2026-10-31): both designs are **not frozen**. No paid collection and no confirmatory run.
