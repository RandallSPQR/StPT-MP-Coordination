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

**Not yet established: the box identity check** (`pipeline/lineup/pod/serve_lineup.sh`). It covers:
- `--hash` re-read of the weights;
- tokenizer facts and template agreement on the box;
- 8-request bit-identity;
- exact-fallback consistency.

It needs one GPU pod per model. Until it runs, "deterministic kernels" on this stack is a requirement, not a result.

## Change policy

Any change to a pinned value, a serializer or the serving stack makes a new version (v1.2.1 or later) and an entry in
`AMENDMENTS.md`. Each study re-pins explicitly. Studies never float on a branch.
