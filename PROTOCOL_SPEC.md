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
    T (D23): primary, per model, set so that a lone 3/4 report gives its Bayesian posterior 0.75 (Llama-3.1-8B 0.50,
    Gemma-2-9B 2.15, Qwen3.5-9B 1.78, Gemma-3-27B 10.40, Qwen3.5-27B 2.88); secondary, T = 1 on a seeded share.
  - Missing Piece samples votes at temperature 0.6, top-p 0.9.
- **Probes:** forked, non-returning branches in both studies, with byte-identity checks.
- **Answer instruction (D20, 2026-11-02):** every vote prompt in both studies ends with a strict one-letter
  instruction. Missing Piece's vote prompt uses StPT W3's sentence: "Reply with exactly one letter, A or B, and
  nothing else." Under StPT S3, this instruction cut non-A/B mass at the vote question from 0.38–0.59 (three models,
  plain "Answer with a single letter") to at most 0.003.
- **StPT board wordings:** locked verbatim in the section "StPT board wordings" (D20, D21).
- **StPT letter mapping (D20):** balanced within each server-start block (seeded permuted blocks; recorded per call).

## StPT board wordings (locked 2026-11-02; D20, D21)

Source of truth: StPT `stpt/prompts.py` (`render`, `TEMPLATES`, `QUESTIONS`). Each wording's definition hash is the
sha256 of `{"labels": TEMPLATES[w], "questions": QUESTIONS[w]}` as canonical JSON (sorted keys, no spaces). Any change
to a wording is a new wording and must pass S3 again. The lines are always in this order; the letter listed first,
the letter mapping, the own vote, the contact votes and their order are randomised per call and recorded.

| wording | valid on the held-out set for | use |
|---|---|---|
| W3 | all five models | primary: the only wording in the pooled confirmatory tests |
| W4 | Llama-3.1-8B, Qwen3.5-9B, Gemma-3-27B, Qwen3.5-27B (fails Gemma-2 own vote 0.829) | per-model consistency check only |
| W5 | Llama-3.1-8B, Gemma-2-9B, Gemma-3-27B, Qwen3.5-27B (fails Qwen3.5-9B own report 0.896) | per-model consistency check only |
| W6 | Gemma-3-27B, Qwen3.5-27B (fails Llama own report 0.771, Gemma-2 own vote 0.600, Qwen3.5-9B own report 0.842) | per-model consistency check only |

Retired: W1 (Qwen3.5-9B own report 0.867, round 1) and W2 (Gemma-2 own vote 0.683, round 2).

#### W3  (definition sha256 `1923475336de7e372eb18d7dd452c4266d82af325d44c6deab1fe94a2cd678ec`)

Live board, informed agent (the letter listed first, the letter mapping, the votes and the contact order are filled per call):

```text
You are one of 100 people answering the same question. Each person has their own private report (or none) and can see the current votes of a few of the others.

Question: {QUESTION with the options in listed order}

The two options are:
{FIRST LETTER}: {OPTION listed first}
{SECOND LETTER}: {OPTION listed second}

Your private report: "{REPORT TEXT}"
Your current vote: {OWN VOTE LETTER}
Votes currently shown by the 4 people you are connected to: {L1}, {L2}, {L3}, {L4}

Which option do you vote for now? Reply with exactly one letter, A or B, and nothing else.
```

- Blind board: the contacts line is replaced by `You cannot see anyone else's vote.`
- Uninformed agent: the report line is `Your private report: ` followed by the scenario's no-report sentence, unquoted.
- vote: `Which option do you vote for now? Reply with exactly one letter, A or B, and nothing else.`
- own_evidence: `Question about your private report: which option does your private report say? Reply with exactly one letter, A or B, and nothing else.`
- own_vote: `Question about your own vote: which option is your current vote? Reply with exactly one letter, A or B, and nothing else.`
- contact_majority: `Question about the people you are connected to: which option do more of them currently vote for? Reply with exactly one letter, A or B, and nothing else.`

#### W4  (definition sha256 `2b2228c32a3eb22ad37d242bea19d898676afc27c25363ad32c02a5abd4f88bf`)

Live board, informed agent (the letter listed first, the letter mapping, the votes and the contact order are filled per call):

```text
This is a group decision among 100 people who all answer the same question. Each of them may hold a private report of their own (some hold none), and each can see how a few of the others are voting at the moment.

The question: {QUESTION with the options in listed order}

Options:
{FIRST LETTER}: {OPTION listed first}
{SECOND LETTER}: {OPTION listed second}

Private report you hold: "{REPORT TEXT}"
Your vote at the moment: {OWN VOTE LETTER}
Your 4 connections' votes at the moment: {L1}, {L2}, {L3}, {L4}

Which option is your vote now? Answer with one letter only, A or B, and no other text.
```

- Blind board: the contacts line is replaced by `No one else's vote is visible to you.`
- Uninformed agent: the report line is `Private report you hold: ` followed by the scenario's no-report sentence, unquoted.
- vote: `Which option is your vote now? Answer with one letter only, A or B, and no other text.`
- own_evidence: `Question about the private report you hold: which option does that report say? Answer with one letter only, A or B, and no other text.`
- own_vote: `Question about your vote at the moment: which option is it? Answer with one letter only, A or B, and no other text.`
- contact_majority: `Question about your connections: which option are more of them voting for at the moment? Answer with one letter only, A or B, and no other text.`

#### W5  (definition sha256 `d369b362e7410cfbdbbc7b851efcf7334d3d5aa801b4c280d718094855f1f9bd`)

Live board, informed agent (the letter listed first, the letter mapping, the votes and the contact order are filled per call):

```text
You are taking part in a vote among 100 people on one question. Some people were given a private report that only they can see, and everyone can see the votes of the few people they are linked to.

Question to decide: {QUESTION with the options in listed order}

Choices:
{FIRST LETTER}: {OPTION listed first}
{SECOND LETTER}: {OPTION listed second}

The report you received privately: "{REPORT TEXT}"
How you are voting right now: {OWN VOTE LETTER}
How the 4 people linked to you are voting right now: {L1}, {L2}, {L3}, {L4}

Which choice do you vote for now? Respond with a single letter, A or B, and nothing more.
```

- Blind board: the contacts line is replaced by `You cannot see how anyone else is voting.`
- Uninformed agent: the report line is `The report you received privately: ` followed by the scenario's no-report sentence, unquoted.
- vote: `Which choice do you vote for now? Respond with a single letter, A or B, and nothing more.`
- own_evidence: `Question about the report you received privately: which choice does that report support? Respond with a single letter, A or B, and nothing more.`
- own_vote: `Question about how you are voting right now: which choice is your vote? Respond with a single letter, A or B, and nothing more.`
- contact_majority: `Question about the people linked to you: which choice are more of them voting for right now? Respond with a single letter, A or B, and nothing more.`

#### W6  (definition sha256 `1818454a552858a25744bd54e67bf18a3dc5c53d9c2e29f61c47f3c8e990193f`)

Live board, informed agent (the letter listed first, the letter mapping, the votes and the contact order are filled per call):

```text
100 people are each answering the same question. Each has a private report or none, and each can see the latest votes of the few people they are connected to.

The question is: {QUESTION with the options in listed order}

Answer options:
{FIRST LETTER}: {OPTION listed first}
{SECOND LETTER}: {OPTION listed second}

Your own report (no one else sees it): "{REPORT TEXT}"
Your own vote so far: {OWN VOTE LETTER}
Latest votes of your 4 contacts: {L1}, {L2}, {L3}, {L4}

What is your vote now? Give exactly one letter, A or B, with nothing else.
```

- Blind board: the contacts line is replaced by `You see no one else's vote.`
- Uninformed agent: the report line is `Your own report (no one else sees it): ` followed by the scenario's no-report sentence, unquoted.
- vote: `What is your vote now? Give exactly one letter, A or B, with nothing else.`
- own_evidence: `Question about your own report: which option does it say? Give exactly one letter, A or B, with nothing else.`
- own_vote: `Question about your own vote so far: which option is it? Give exactly one letter, A or B, with nothing else.`
- contact_majority: `Question about your contacts: which option do more of them vote for? Give exactly one letter, A or B, with nothing else.`

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

**Measured (StPT S4, 2026-11-02; A16).** Within a start, readouts are bit-identical (all five models). Between starts:
S_p (vote-probability shift at p = 0.5, T = 1) is 0.018–0.021 for Llama-3.1-8B, Qwen3.5-9B and Qwen3.5-27B, 0 for
Gemma-2-9B, and 0.049–0.056 for Gemma-3-27B. **StPT's tripwire is triggered: blocking by start is mandatory for every
StPT analysis.** Missing Piece's (0.05) is treated as triggered too (owner, D22): blocking by start is mandatory for
every Missing Piece analysis. Starts fall into a few discrete numerical states;
systematic per-start offsets are negligible.

## Matched bridge interface (after both core studies)

The bridge holds these fixed across its two arms, on a complete group of 5:
- the agents (pinned models);
- the evidence allocation (same scenarios, same assignment of items to positions);
- the task;
- the decision rule (final private vote).

The two arms are the Missing Piece talk protocol and a vote-trace protocol (each agent sees the other four agents'
current votes over the same number of rounds). Differences to record: memory (the vote-trace memory policy is to be
specified), bandwidth, and attribution (labelled or anonymous votes, to be specified).
