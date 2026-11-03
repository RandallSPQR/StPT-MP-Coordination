# Sealed files and openings (D27)

A sealed file holds point estimates that must not inform design or sample-size choices before the freeze. It is written
once by analysis code that never prints it. Its sha256 is recorded here when it is written. It is opened only at the
owner's request: the opening script appends a row to the openings table **before** it decodes the file.

## Sealed files

| file | written | sha256 | contents |
|---|---|---|---|
| StPT `gates/S8/sealed/s8_sealed.json.gz.b64` | 2026-11-03 00:34 UTC | `e6b1bc7ac35d28591428efc8dbc9836f8b9d6ff98b2cb55fcf76fa72f16a95a8` | S8 pilot point estimates: live − blind; persistence (ITT, exposed-only, social, self); per model, per setting, pooled. Not opened. |
| StPT `gates/S10/sealed/s10_predictions.json.gz.b64` | 2026-11-03 05:49 UTC | `1a73a88d2a5efb4bb351b2078f59cb405b1719b7063518aeb17a5d13858b58c2` | S10 pre-registered predictions (kernels from S8 at T, 200 draws × 5 runs, on the 61 confirmatory clusters): pooled and per-model predictive distributions of both estimands, per-cluster predictions. Proposed to be unsealed into the OSF registration at posting. Not opened. |

## Openings

| date (UTC) | file | requested by | reason |
|---|---|---|---|
| 2026-11-03 06:22 | `gates/S10/sealed/s10_predictions.json.gz.b64` | owner (D45) | inclusion in the pre-registration at freeze (D45; after the freeze commit 17063f58c9fb6f9064477c555c5e42200e70115f) |
| 2026-11-03 06:22 | `gates/S8/sealed/s8_sealed.json.gz.b64` | owner (D45) | inclusion in the pre-registration at freeze (D45; after the freeze commit 17063f58c9fb6f9064477c555c5e42200e70115f) |
