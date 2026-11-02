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
| D14 | 2026-11-01 | both | **Server-restart drift: option (a), accepted on pipeline v1.2.0 with blocking** (prospective; no study data exists). (1) Every run manifest records the server-start ID. (2) Block by server start: all paired contrasts run within the same start (StPT live vs blind, and pulsed vs unpulsed branches; Missing Piece's four cells per scenario); conditions are balanced across starts; the start ID is a blocking factor in both analyses. (3) StPT S4 reports two quantities: the within-start repeat SD (existing 0.3-nat limit) and the between-start shift, both also in vote probability at the operating point (p near 0.5). (4) Between-start variance is propagated into kernel uncertainty and into the power tier tables. (5) Tripwire: if the between-start shift in vote probability exceeds half the reference effect (StPT 0.025; Missing Piece 0.05), blocking becomes mandatory for every analysis and the trigger is logged in `AMENDMENTS.md`. | owner, after the v1.2.1 candidate failed (A6) |
| D15 | 2026-11-01 | StPT | S2 evidence conventions approved **as provisional values**: sources right 3 in 5, 3 in 4, 9 in 10 (LR 1.5, 3, 9), equal weights; half the agents informed (one report each); pooled margin 3.0 log-odds. | owner, after StPT S2 |
| D16 | 2026-11-01 | StPT | Confirmatory bank: **no shared-report variants** (shared-report variants stay in the development bank only). | owner, after StPT S2 |
| D17 | 2026-11-01 | StPT | **Difficulty selection rule, fixed before any difficulty result.** Difficulty is chosen by **blind-board accuracy only, never by the size of any live-board effect**. The informed fraction stays at a middle level (it is rung 2's dose factor). Challenge-set boards stay a labelled stratum, not primary. CPU first: a difficulty grid (informed fraction × source strength × challenge-set share) with a family of proxy kernels anchored on SPT's measured values; report blind accuracy and its spread across kernels per cell; identify the region where blind accuracy falls in a pre-stated band for most kernels. After S3–S5, re-run with measured kernels; S8 pilots 2–3 settings from the region. | owner, after StPT S2 |
| D18 | 2026-11-01 | StPT | **Difficulty set by simulated power, not by hand** (owner's addendum). The difficulty region is the one that maximises simulated power to detect a board effect of ±0.05 in both directions (aggregation and cascade), from the proxy-kernel simulation, using blind-board accuracy and the board-to-board variance of the paired contrast; never a simulated live effect (D17). **Chosen, at informed fraction 0.5: sources {3/4, 9/10}** (score 3.1 = 75th percentile over 81 proxy kernels of clusters per model for 80% power in the worse direction, pooled 5-model test, α 0.025). **Region for S8:** {3/4, 9/10} and the provisionally approved {3/5, 3/4, 9/10} (3.6, within 25% of the best). **Rationale:** stronger sources anchor informed agents and cut the board-to-board variance of the contrast (SD 0.044 vs 0.049) and its heavy tail (p90 6.8 vs 35 for {3/5}); both directions are feasible (blind accuracy 0.50–0.64). Unconstrained optimum (reported only): f 0.7 with {3/4, 9/10}. Proxy variances are optimistic in absolute terms; only the ranking is used. Re-run with measured kernels after S3–S5. Recorded **before any live data**. Record: StPT `gates/S2/difficulty/POWER_REPORT.md` (commit 285330b). | owner's rule; result of the pre-stated computation |
| D19 | 2026-11-01 | StPT | **Pre-registered secondary analysis: board effect by informed status.** Uninformed agents (transmission) vs informed agents (social override); membership fixed by the allocation before the run; same paired, start-blocked model as the primary; intervals, no power target. The all-agent primary outcome is unchanged. D18 accepted by the owner; the confirmatory source set {3/4, 9/10} at informed fraction 0.5 is now stated in the specs, the conventions and the brief. Note for S9: proxy-kernel cluster counts are optimistic (no restart drift, wording variance, call noise or N = 100 graph spread); tier sizes come from measured kernels with every variance source included. Record: StPT `analysis/SECONDARY_INFORMED_SPLIT.md`, `gates/S9/NOTES.md` (commit 4d38869). | owner, after D18 |
| D20 | 2026-11-02 | both | **Wording follow-ups before S4** (owner, after StPT S3 round 2; prospective, no study outcome exists). (1) **Held-out wording check:** a fresh comprehension set (new seed, development bank, round 1's size and type balance) runs W3 and three new W3 variants (W4–W6: different phrasing and label text, same structure, report labelled and quoted in place, strict one-letter instruction). Rule fixed before any call: a wording is valid if all five models pass all three types at ≥ 0.90 on the held-out set; W3 must pass to remain the primary wording; if W3 fails, S4 does not start. (2) **Prompt sensitivity redefined:** with few valid wordings, wording is **not a random factor**. The board effect and pulse persistence are estimated per valid wording; the consistency criterion is same sign with overlapping intervals. W2's structural failure (Gemma-2 own vote 0.683 when the report sits next to the question) is reported as a sensitivity finding. (3) **Pre-registered sensitivity analysis:** both confirmatory contrasts re-estimated excluding Qwen3.5-9B (context interference: own-report accuracy 0.82 when its own vote disagrees with its report). (4) **Letter mapping balanced within each server-start block.** (5) The valid wordings' exact text is locked in `PROTOCOL_SPEC.md`; Missing Piece's vote prompt uses the strict one-letter instruction. S4 approved to run straight after the held-out check with the wordings that pass. | owner, after StPT S3 round 2 |

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
- **System messages.** Gemma-2's official template refuses a system role. The pipeline's gemma2 serializer folds system
  text into the first user turn (turn 0), as the earlier Gemma-2 work did. Template agreement is therefore checked on
  system-free conversations, where all five models agree (StPT S0). Whether StPT and MP prompts use a system message is
  decided at their comprehension gates.
