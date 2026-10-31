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

## Post-outcome deviations

None.
