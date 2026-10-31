# Decision register

Status: **accepted** (owner decided, with date), **proposed** (awaiting the owner), **unresolved** (open).
Superseded entries are struck through and kept.

## Accepted

| id | date | study | decision | source |
|---|---|---|---|---|
| D1 | 2026-10-29 | both | Models: Llama-3.1-8B-Instruct, Gemma-2-9B-IT, Gemma-3-27B-IT and a Qwen pair (pencilled). All open weights, self-hosted on one pinned shared pipeline, batch size 1, deterministic kernels, exact logprobs. GPT-4o-mini is out of the confirmatory set. | both briefs |
| D2 | 2026-10-30 | both | Estimand: each confirmatory contrast is ONE pooled test of the average effect across the five models (models as fixed strata). Per-model estimates are secondary. | both briefs |
| D3 | 2026-10-30 | StPT | Two confirmatory contrasts: live minus blind (ordinary agents' final support for the correct option), and pulse persistence. Holm across the two. A separately powered TOST for each. | StPT brief r3 |
| D4 | 2026-10-30 | StPT | Core = blind, live and randomised display pulse. Committed nodes, prerecorded boards and reversal branches are rung 1b. No cascade claim in the core. | StPT brief r3 |
| D5 | 2026-10-30 | MP | One confirmatory contrast (the knowledge main effect). Knowledge × verifier is secondary, two-sided. | MP brief r2 |
| D6 | 2026-10-30 | both | Power is by independent cluster count. Tiers come from the simulation; the owner picks a tier. | both briefs |
| D7 | 2026-10-31 | both | Coordination location = this repository, `RandallSPQR/StPT-MP-Coordination` (was P5). | the owner created the repository and had the record pushed to it |
| D8 | 2026-10-31 | both | Qwen pair = Qwen3.5-9B and Qwen3.5-27B, dense, thinking off (was P1). | owner approval in the StPT session |
| D9 | 2026-10-31 | both | One serving stack for all five models: vLLM 0.19.1, torch 2.10.0, transformers 5.5.3; batch size 1, raw logprobs, prefix cache off (was P3). | owner approval in the StPT session |
| D10 | 2026-10-31 | both | All five models served in bf16, including Gemma-2-9B (was P4). | owner approval in the StPT session |
| D11 | 2026-10-31 | StPT | GPU spend approved for S0's box identity check (one pod, five models in sequence, 4 h hard cap). | owner approval in the StPT session |
| D12 | 2026-10-31 | both | Serving GPU type = NVIDIA RTX PRO 6000 Blackwell Server Edition (determinism is established on it); not the A100. | owner, after a cost/availability comparison |
| D13 | 2026-10-31 | both | Restart drift: option (b), test candidate settings for a v1.2.1 serving stack. | owner approval in the StPT session |

## Proposed (Step 0, 2026-10-31; awaiting the owner)

| id | study | proposal | why |
|---|---|---|---|
| ~~P1~~ | both | ~~Qwen pair = Qwen3.5-9B and Qwen3.5-27B, thinking off.** | Both dense, with one shared architecture (no mixture-of-experts confound). SPT's Qwen-9B was Qwen3.5-9B (continuity). |
| P2 | both | **Pipeline = sae-ethos-pipeline v1.2.0** (`pipeline.yaml`), locked. Its Qwen pair, stack and dtype are accepted (D8-D10). | Built at Step 0 as the briefs require. |
| ~~P3~~ | both | ~~One serving stack for all five models: vLLM 0.19.1**, torch 2.10.0, transformers 5.5.3. Batch size 1, raw logprobs, prefix cache off, bf16. | vLLM 0.11.0 (the SAE studies' pin) cannot serve Qwen3.5. One stack keeps kernels comparable across models. |
| ~~P4~~ | both | ~~All five models are served in bf16, including Gemma-2-9B. | The closed SAE study's fp32 serving belongs to that study's instrument. |
| ~~P5~~ | both | ~~Coordination location = this repository.~~ Accepted as D7 (2026-10-31). | |
| P6 | StPT | **SPT release for the S0 replay = public `social-phase-transitions-public` tag `v1.0.1`**, commit `c37a91d2d2561fdf78fc61af500ea000aac3ea2f` (a snapshot of private `3a26daf` plus documentation commits; Zenodo 10.5281/zenodo.23821962). | It is the released, DOI-archived state. No new SPT tag was needed. |

## Unresolved (from the briefs; none blocks S0)

- **Both:** budget cap (set before the power simulation) and per-stage caps.
- **StPT:**
  - Smallest meaningful effects and equivalence bounds. Proposed references: 0.05 support, ±0.03.
  - Pulse time, duration, r, k and the persistence window.
  - Operational horizon, initial-vote rule, decision-noise T, tie rule.
  - Evidence conventions, pooled margin, informed fraction, bank sizes.
  - Power tier.
- **MP:**
  - Rounds and length band.
  - Pooled margin and likelihood-ratio conventions.
  - Bank sizes.
  - Bridge memory policy and attribution.
  - Power tier.
- **Server-restart drift.** The owner chose option (b): test settings for a v1.2.1 serving stack (2026-10-31).
  Result (StPT `gates/S0/S0_REPORT.md` 4b):
  - **C2e** = `VLLM_BATCH_INVARIANT=1` + `--attention-backend FLASH_ATTN` + `--enforce-eager` is bit-identical across
    cold starts for Llama-8B and Qwen3.5-9B. The other three settings tested are not.
  - Its cost: +84% / +34% per call.
  - Open: confirm C2e on all five models, then release it as v1.2.1 (both studies re-pin).

- **System messages.** Gemma-2's official template refuses a system role. The pipeline's gemma2 serializer folds system
  text into the first user turn (turn 0), as the earlier Gemma-2 work did. Template agreement is therefore checked on
  system-free conversations, where all five models agree (StPT S0). Whether StPT and MP prompts use a system message is
  decided at their comprehension gates.
