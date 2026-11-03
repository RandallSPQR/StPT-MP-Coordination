# Sealed files and openings (D27)

A sealed file holds point estimates that must not inform design or sample-size choices before the freeze. It is written
once by analysis code that never prints it. Its sha256 is recorded here when it is written. It is opened only at the
owner's request: the opening script appends a row to the openings table **before** it decodes the file.

## Sealed files

| file | written | sha256 | contents |
|---|---|---|---|
| StPT `gates/S8/sealed/s8_sealed.json.gz.b64` | 2026-11-03 00:34 UTC | `e6b1bc7ac35d28591428efc8dbc9836f8b9d6ff98b2cb55fcf76fa72f16a95a8` | S8 pilot point estimates: live − blind; persistence (ITT, exposed-only, social, self); per model, per setting, pooled. Not opened. |

## Openings

| date (UTC) | file | requested by | reason |
|---|---|---|---|
