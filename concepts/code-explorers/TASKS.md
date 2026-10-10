# Tasks

## Task 1 — Build the remaining artifacts

Same rigor as `denoising_loop.html`: pinned source (diffusers v0.40.0), every line verified character-for-character, `VERIFICATION.md`'s checklist run before shipping, `MISTAKES.md` read first and added to when something new is caught. No GitHub folder/Pages work here — just build, verify, publish as a Claude Artifact, commit the `.html` to `concepts/code-explorers/` and add a row to this folder's `README.md` table.

Remaining, in the X-Ray's own route order:

1. DDIM's reverse step — `scheduling_ddim.py` L440–495 (Stage B; the black box `denoising_loop`'s derivation 3 deferred)
2. Cross-attention — `attention.py` L960–1082 + `attention_processor.py` L1136–1160 (Stage D, the X-Ray's "deepest")
3. UNet forward (skim) — `unet_2d_condition.py` L979–1235 (Stage C)
4. `encode_prompt` — `pipeline_stable_diffusion.py` L332–500 (Stage A)
5. Processor registry — `attention.py` L41–96 (Stage D)

## Task 2 — GitHub Pages

Repo-hosting layer only, no artifact content work. Enable Pages on this repo (separate from the `rishavghosh605.github.io` portfolio repo, served at a sub-path, no conflict), organize one folder per stage, write each folder's README to link both the Claude Artifact and the Pages-served HTML.
