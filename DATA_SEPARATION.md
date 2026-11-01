# Development / confirmatory separation record

| date | study | bank / dataset | split | who may read it | status |
|---|---|---|---|---|---|
| 2026-11-01 | StPT | development bank: 16 scenario specs + 72 allocations (StPT `gates/S2/banks/development/`, commit 0822269) | development | every StPT gate before S10 | committed before any model call; hashes in `gates/S2/banks/MANIFEST.json` (file sha256 4de05fa2e423416c…) |
| 2026-11-01 | StPT | confirmatory bank: 12 scenario specs, no allocations (StPT `gates/S2/banks/confirmatory/`, commit 0822269) | confirmatory | specs only; allocations are drawn after S9 by the fixed rule `conf-alloc|<scenario_id>|<k>` | stories disjoint from development; no model call on these specs before the confirmatory run |

Rules (both briefs):
- Development and confirmatory banks are written and committed separately, before any model call on them.
- A confirmatory scenario is checked only on criteria that do not involve group outcomes. None is selected for
  producing group failure.
- Each bank's file hashes are entered here when the bank is committed, and the hashes are posted at the freeze.
