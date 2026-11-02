# Sealed files and openings (D27)

A sealed file holds point estimates that must not inform design or sample-size choices before the freeze. It is written
once by analysis code that never prints it. Its sha256 is recorded here when it is written. It is opened only at the
owner's request: the opening script appends a row to the openings table **before** it decodes the file.

## Sealed files

| file | written | sha256 | contents |
|---|---|---|---|

## Openings

| date (UTC) | file | requested by | reason |
|---|---|---|---|
