# Development / confirmatory separation record

| date | study | bank / dataset | split | who may read it | status |
|---|---|---|---|---|---|
| 2026-11-01 | StPT | development bank: 16 scenario specs + 72 allocations (StPT `gates/S2/banks/development/`, commit 0822269) | development | every StPT gate before S10 | committed before any model call; hashes in `gates/S2/banks/MANIFEST.json` (file sha256 4de05fa2e423416c…) |
| 2026-11-01 | StPT | confirmatory bank: 12 scenario specs, no allocations (StPT `gates/S2/banks/confirmatory/`, commit 0822269; source set changed to {3/4, 9/10} by D18, commit 4d38869, new hashes in MANIFEST, file sha256 74c1e2faaf801b21…) | confirmatory | specs only; allocations are drawn after S9 by the fixed rule `conf-alloc|<scenario_id>|<k>` | stories disjoint from development; no model call on these specs before the confirmatory run |

| 2026-11-03 | MP | development bank: 12 stories → 24 specs + 8 labelled challenge variants (MP `gates/S1/banks/development/`, commit 829691b) | development | every MP gate before the freeze (S3 comprehension, S4 pilot, S5 grading, solo pooled check) | committed before any model call; hashes in MP `gates/S1/banks/MANIFEST.json` (file sha256 4458390c097717e7…) |
| 2026-11-03 | MP | confirmatory bank: 26 stories → 52 specs, no challenge variants (MP `gates/S1/banks/confirmatory/`, commit 829691b) | confirmatory | specs only; before the confirmatory run the only model calls on them are S2's prior check and content-refusal screen (no group outcome) | stories, option texts, sources and names disjoint from development; never selected on a group outcome |
Rules (both briefs):
- Development and confirmatory banks are written and committed separately, before any model call on them.
- A confirmatory scenario is checked only on criteria that do not involve group outcomes. None is selected for
  producing group failure.
- Each bank's file hashes are entered here when the bank is committed, and the hashes are posted at the freeze.
