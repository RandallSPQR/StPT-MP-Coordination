# Shared protocol specification

**Version: 0.0 (skeleton).** It records what the two briefs already fix in common and holds slots that the studies
fill at their own gates. Nothing here is frozen.

## Evidence schema

**Schema version: unassigned.** The first version is written at StPT gate S2 and Missing Piece gate S1, before any model
sees a scenario. Both briefs already fix its content. Per scenario:
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

## Matched bridge interface (after both core studies)

The bridge holds these fixed across its two arms, on a complete group of 5:
- the agents (pinned models);
- the evidence allocation (same scenarios, same assignment of items to positions);
- the task;
- the decision rule (final private vote).

The two arms are the Missing Piece talk protocol and a vote-trace protocol (each agent sees the other four agents'
current votes over the same number of rounds). Differences to record: memory (the vote-trace memory policy is to be
specified), bandwidth, and attribution (labelled or anonymous votes, to be specified).
