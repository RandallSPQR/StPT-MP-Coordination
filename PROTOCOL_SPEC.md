# Shared protocol specification

**Version: 0.0 (skeleton).** It records what the two briefs already fix in common and holds slots that the studies
fill at their own gates. Nothing here is frozen.

## Evidence schema

**Schema version: 1.0** (2026-11-01): `schema/evidence_schema_v1.0.json`. It was written at StPT gate S2, and the
reference implementation is StPT `stpt/evidence.py`. Missing Piece adopts it at its S1. Its study-specific fields
(briefings, hearsay report, verifier) sit under `story` and `evidence_model` without changing the required fields.
A change to the required fields is a new schema version and an entry in `AMENDMENTS.md`.

How StPT uses it (provisional conventions, StPT `gates/S2/CONVENTIONS.md`): every evidence item is a report from a
source with a stated accuracy in plain frequencies. Its designed LR is a/(1 − a) toward the option it favours. Both briefs already fix its content. Per scenario:
- a hidden state S ∈ {A, B} and a prior (0.5);
- for every evidence item, a designed likelihood ratio P(item | A) / P(item | B);
- a dependence structure: shared items counted once; declared-dependent items grouped and counted once per group;
  all others conditionally independent given S;
- the pooled-evidence benchmark: the prior log-odds plus the summed log likelihood ratios of the distinct items held;
- a prespecified margin.

Objective strength (the design) is kept separate from each model's responsiveness to wording, which is measured.
Development and confirmatory banks are separate. A selected minority-correct profile is labelled a conditional
challenge set.

## Model calls (both studies; pipeline v1.2.0)

- **Prompt:** the canonical serializer for the model's family (`modelcfg.serializer()`). Thinking is off for Qwen.
  The server's chat template is never used.
- **Binary readout:** `lineup.readout`. It gives exact log P(A) and log P(B), the non-option mass, and the top 20 for
  audit. How a study turns a readout into a vote is the study's protocol:
  - StPT samples from the A/B probabilities at decision noise T (logits / T), with seeded draws; never argmax.
  - Missing Piece samples votes at temperature 0.6, top-p 0.9.
- **Probes:** forked, non-returning branches in both studies, with byte-identity checks.
- **Answer instruction (D20, 2026-11-02):** every vote prompt in both studies ends with a strict one-letter
  instruction. Missing Piece's vote prompt uses StPT W3's sentence: "Reply with exactly one letter, A or B, and
  nothing else." Under StPT S3, this instruction cut non-A/B mass at the vote question from 0.38–0.59 (three models,
  plain "Answer with a single letter") to at most 0.003.
- **StPT board wordings:** locked here, verbatim, once the held-out check has run (D20; section "StPT board wordings").
- **StPT letter mapping (D20):** balanced within each server-start block (seeded permuted blocks; recorded per call).

## Server starts (D14, 2026-11-01)

Under pipeline v1.2.0, logprobs are bit-identical within one vLLM server start, but can move by 1–2 bf16 steps (up to
0.25 nats) between starts (StPT S0, sections 1b and 4c). Both studies therefore:
- record a **server-start ID** in every run manifest: pod id + start index + start time;
- run **every paired contrast within one start**:
  - StPT: live and blind runs, and the pulsed and unpulsed branches of each saved state;
  - Missing Piece: the four cells of a scenario;
- balance conditions across starts, and use the start ID as a **blocking factor** in the analysis;
- carry between-start variance into kernel uncertainty (StPT) and into the power tier tables (both).

**Tripwire.** If the between-start shift in vote probability at p ≈ 0.5 exceeds half the reference effect (StPT 0.025,
Missing Piece 0.05), blocking becomes mandatory for every analysis, not only the paired contrasts, and the trigger is
logged in `AMENDMENTS.md`. StPT gate S4 measures this shift on the shared pipeline.

## Matched bridge interface (after both core studies)

The bridge holds these fixed across its two arms, on a complete group of 5:
- the agents (pinned models);
- the evidence allocation (same scenarios, same assignment of items to positions);
- the task;
- the decision rule (final private vote).

The two arms are the Missing Piece talk protocol and a vote-trace protocol (each agent sees the other four agents'
current votes over the same number of rounds). Differences to record: memory (the vote-trace memory policy is to be
specified), bandwidth, and attribution (labelled or anonymous votes, to be specified).
