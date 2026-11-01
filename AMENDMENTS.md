# Amendment and deviation log (chronological)

Two kinds of entry, never mixed:
- **Prospective amendment:** decided before any outcome it could affect was available.
- **Post-outcome deviation:** decided after relevant outcomes were available. It is labelled as such and reported
  separately.

## Prospective amendments

| # | date | study | change | outcome data available at the time? |
|---|---|---|---|---|
| A1 | 2026-10-31 | both | Pipeline v1.2.0 created and locked: Qwen3.5 pair added, one vLLM 0.19.1 serving stack, exact binary readout, identity block (`PIPELINE.md`). | none: no study data exists |
| A2 | 2026-10-31 | both | Record update, no design change. Pipeline v1.2.0 published on GitHub (lightweight tag at the pinned commit; `main` merge `4632409`, same tree). This record published at `93a9ee7`. Coordination location accepted (D7). StPT re-pins this record at the commit that adds this entry. | none: no study data exists |

| A3 | 2026-10-31 | both | Decisions D8–D11 accepted. **Known defect in pipeline v1.2.0, not fixed in place:** `pipeline/lineup/pod/serve_lineup.sh` runs `python -m lineup.serve`, which cannot import `lineup` from an installed copy. The serving command itself is correct: the `lineup-serve-cmd` console script works. StPT's S0 driver (`gates/S0/gpu/box_identity.sh`) performs the same steps with the pipeline's own functions and console scripts. A fix would be v1.2.1. | none: no study data exists |
| A4 | 2026-10-31 | both | Record only, no change to the pipeline. Box identity check passed 5/5 (StPT S0). It ran on an RTX PRO 6000 Blackwell; the owner chose it as the fallback when no A100 was available. The first pod's 27B downloads failed on a full disk, and the 27B models were re-run on a second pod. Finding: no bit-identity across server restarts (see `DECISIONS.md`, unresolved). | none: no study data exists |
| A5 | 2026-10-31 | both | Record only. Decisions D12–D13. Restart test: C2e removes cross-start drift for the two screening models (StPT S0 4b). No pipeline change yet: confirmation on all five models and a v1.2.1 release await the owner. | none: no study data exists |
| A6 | 2026-11-01 | both | Record only. The pipeline v1.2.1 candidate (branch `release/v1.2.1`, `ffc4902a1db0`) failed its confirmation run and was **rejected, not tagged**; the branch is marked REJECTED. Both studies stay on v1.2.0. See StPT S0 report 4c. | none: no study data exists |
| A7 | 2026-11-01 | both | **Prospective:** server-start blocking (D14). Both studies stay on v1.2.0. Manifests record the server-start ID. Paired contrasts run within one start. Start is a blocking factor. StPT S4 reports within- and between-start noise. Between-start variance enters kernel uncertainty and the power tiers. Tripwire at half the reference effect. Both briefs' protocol and analysis sections updated to match. | none: no study data exists |
| A8 | 2026-11-01 | both | Evidence schema v1.0 written (StPT S2). StPT development bank committed and confirmatory specs committed, with provisional conventions awaiting the owner (StPT `gates/S2/CONVENTIONS.md`). | none: no model has seen a scenario |
| A9 | 2026-11-01 | StPT | D15–D17: S2 conventions accepted as provisional; no shared-report variants in the confirmatory bank; difficulty chosen by blind-board accuracy only (rule fixed before any difficulty result). | none: no model has seen a scenario; no difficulty simulation had run |
| A10 | 2026-11-01 | StPT | D18: difficulty chosen by simulated power (rule from the owner; computation pre-stated in StPT `gates/S2/difficulty/POWER_PLAN.md`): informed fraction 0.5, sources {3/4, 9/10}; S8 region adds {3/5, 3/4, 9/10}. The provisional source set of D15 stays in the development bank until S8. Shared-report variants excluded from the confirmatory bank (confirmed, D16). | none: no live data; CPU proxy simulation only |
| A11 | 2026-11-01 | StPT | D19: secondary analysis by informed status pre-registered. The 12 confirmatory scenario specs now carry the D18 source set {3/4, 9/10} at informed fraction 0.5; the hashes changed (StPT `gates/S2/banks/MANIFEST.json`, file sha256 74c1e2faaf801b21…). No model had seen these specs. The StPT brief states the setting and the split. | none: no live data; no model call on the confirmatory bank |
| A12 | 2026-11-01 | StPT | **Wording revision after S3 round 1 (owner's option a).** Two revised board wordings (W2 structured, report last; W3 labelled and quoted; both with a strict one-letter answer instruction) are tested at S3 round 2 on the development bank. Rule fixed before round 2: a wording is valid only if all five models pass all three comprehension types; the study's wording set (the brief's prompt-sensitivity factor) is the valid wordings, chosen on comprehension only. Round 1 results stay on record (W1 not valid: Qwen3.5-9B own-report 0.867). StPT \`gates/S3/ROUND2_PLAN.md\` (commit 47be3ae). | comprehension results only (development bank); no study outcome exists |

## Post-outcome deviations

| # | date | study | deviation | outcome seen before the decision | original result kept |
|---|---|---|---|---|---|
| DV1 | 2026-11-01 | StPT | **S1, B3 smooth cases (T = 0 random-field avalanches).** Judged on the mean of 10 independent graphs under the 4 SE + 0.005 rule used by B1, B2 and B4, instead of a single graph within 0.02. Reason: the exact curve is an ensemble result, so the single-graph comparison was mis-specified. The d = 6 pass/fail point moves from σ = 3.5 (near-critical) to σ = 4.5. σ = 3.5 is kept as a reported diagnostic (≥ 10 graphs, mean and spread, no pass/fail). Re-run on new seeds. The measured graph-to-graph spreads (0.025 at d = 4 on the steep section; up to 0.19 near critical disorder at d = 6) become inputs to the S9 power simulation. Owner-approved. | yes: B3's smooth cases failed the pre-stated single-graph tolerance (max 0.050) | yes: StPT `gates/S1/S1_REPORT.md`, `results/B3.json`, `diagnostics/B3_realisation_spread.json` |
