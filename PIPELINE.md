# Pinned pipeline release

**sae-ethos-pipeline v1.2.0**, LOCKED.
- Commit: `5ca15f56a71f35d5818d0392f7f3b4acb2d85d1b`. Parent: v1.1.0 `94a785c15ec8`.
- Repository: https://github.com/RandallSPQR/SAE-Ethos-Calibration (`pipeline/lineup/`, `RELEASE_NOTES.md`).
- Machine-readable: `pipeline.yaml`.

**Publication status (2026-10-31): published.**
- Branch `release/v1.2.0` and tag `v1.2.0` are on GitHub, both at `5ca15f56a71f`.
- `main` took it by merge commit `4632409d06fa` (PR #16), whose tree is identical to the tagged commit.
- The tag is lightweight: it points at the commit itself. The annotated tag object `aedd002b6a20` made at build time
  was not published. Pins therefore name the commit, which is what every manifest checks.
- The install line in `pipeline.yaml` resolves. A clean install at the pinned commit passes StPT's
  `check_environment` for all five models.

## Models

| model | revision | weight_hash | chat_template_hash | family | identity |
|---|---|---|---|---|---|
| Llama-3.1-8B-Instruct | 0e9e39f249a1 | 495b73750141622d | e10ca381b1ccc5cf | llama3 | pod-pinned 2026-10-10 |
| Gemma-2-9B-IT | 11c9b309abf7 | 7055bc8c5f456ced | ecd6ae513fe103f0 | gemma2 | pod-pinned 2026-09-18 |
| Gemma-3-27B-IT | 005ad3404e59 | 34f745e7c6ef5b7b | 7de1c58e208eda46 | gemma3 | pod-pinned 2026-09-29 |
| Qwen3.5-9B, thinking off (pencilled) | c20223623576 | 4b570a1e6e5db4b9 | a4aee8afcf2e0711 | qwen3_5 | metadata-pinned 2026-10-31 |
| Qwen3.5-27B, thinking off (pencilled) | fc05daec18b0 | 5e536c8708a9430b | a4aee8afcf2e0711 | qwen3_5 | metadata-pinned 2026-10-31 |

**How the metadata pins were made.** Each is the box preflight's identity function applied to the revision's recorded
per-file hashes. The same function reproduces the three pod-measured pins exactly.

**Live check (2026-10-31, HF API).** All five revisions resolve, and every weight_hash and chat_template_hash
recomputes to its pin. Report: `pipeline/lineup/identity_reports/offline_2026-10-31.json` in the pipeline repository.

## Runtime

- One serving stack for all five models (its own venv): vLLM 0.19.1, torch 2.10.0, transformers 5.5.3, tokenizers
  0.22.2, huggingface-hub 1.10.2.
- Flags: batch size 1 (`--max-num-seqs 1`), prefix caching off, no prompt chunking, `--logprobs-mode raw_logprobs`,
  `--seed 0`, bf16, text only.
- Environment: `CUBLAS_WORKSPACE_CONFIG=:4096:8`, `PYTHONHASHSEED=0`.

**Box identity check: PASSED, 2026-10-31** (StPT gate S0, `gates/S0/S0_REPORT.md` section 1b).
- GPU: one RTX PRO 6000 Blackwell Server Edition. Stack observed: Python 3.11.13, vLLM 0.19.1, torch 2.10.0+cu128,
  transformers 5.5.3.
- All five models: weights re-hashed from the bytes to their pins (the two Qwen metadata pins included); tokenizer facts
  and template agreement hold; 8 identical requests on 3 prompts are bit-identical within a server start; the exact
  fallback agrees.
- **Not bit-identical across a server restart** for 4 of 5 models: 1–2 bf16 steps on the logit, at most 0.25 nats.
  Gemma-2-9B is identical. The cause is not established (the kernel autotuner is a candidate).
- Determinism is established on this GPU type only.
- **Known defect:** `pipeline/lineup/pod/serve_lineup.sh` cannot import `lineup` (A3). StPT's driver replaces it.

## Studies pinned to this release

- StPT: `pins.yaml` (StPT repository).
- Missing Piece (2026-11-03, D31): `pins.yaml` (MP repository), installed by commit; `mp.manifest.check_environment`
  runs `lineup.manifest.check_pin` for every model before any call. MP S0 (CPU) re-verified the tag → commit mapping and
  every chat_template_hash against the templates used (MP `gates/S0/S0_REPORT.md`).

## Versions by study (D40, 2026-11-03)

- **StPT: v1.2.0**, pinned. All its gate evidence was produced there, and it needs batch size 1 and exact logprobs.
- **Missing Piece: v1.3.0** (P12), once someone with write access applies and tags it. Until then MP stays on v1.2.0
  `strict`.
  - The patch is in MP `proposals/pipeline-v1.3.0/`. Not v1.2.1: that name belongs to the candidate rejected in A6.
  - The default `strict` mode serves exactly as v1.2.0. The throughput modes are opt-in, and MP's S0-GPU validates them
    before any other run.
  - D1 and D9 are amended for MP only.
- **Link check, CPU (done):** v1.3.0-strict and v1.2.0 give identical argv, env, lineup entries, profiles, serializers
  and readout code for all five models (MP `gates/S0/link_check/`). The GPU part is optional and pending.
- **The matched bridge** runs both arms on one pipeline version and one serving mode.

## Change policy

Any change to a pinned value, a serializer or the serving stack makes a new version (v1.2.1 or later) and an entry in
`AMENDMENTS.md`. Each study re-pins explicitly. Studies never float on a branch.
