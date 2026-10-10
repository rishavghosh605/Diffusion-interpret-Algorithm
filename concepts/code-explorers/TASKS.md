# Tasks

## Task 1 — Build the remaining artifacts

Same rigor as `denoising_loop.html`: pinned source (diffusers v0.40.0), every line verified character-for-character, `VERIFICATION.md`'s checklist run before shipping, `MISTAKES.md` read first and added to when something new is caught. No GitHub folder/Pages work here — just build, verify, publish as a Claude Artifact, commit the `.html` to `concepts/code-explorers/` and add a row to this folder's `README.md` table.

Route order (the X-Ray's own), done vs. remaining:

1. ~~DDIM's reverse step~~ — `scheduling_ddim.py` L433–491 (Stage B; the black box `denoising_loop`'s derivation 3 deferred). Done: [`ddim_reverse_step.html`](ddim_reverse_step.html), [live version](https://claude.ai/artifact/1r22ovvRUL1W1iwgMgL8Tb).
2. ~~Cross-attention~~ — `attention_processor.py` L1141–1162, `AttnProcessor.__call__`'s core (Stage D, the X-Ray's "deepest"; superseded the planned `attention.py` L960–1082 + `attention_processor.py` L1136–1160 range once the actual pinned line numbers were checked). Done: [`cross_attention.html`](cross_attention.html), [live version](https://claude.ai/artifact/TbWNMtDZ7QMh3ZNi7SKzju).
3. ~~UNet forward (skim)~~ — `unet_2d_condition.py` L979–1235 (Stage C). Done: [`unet_forward.html`](unet_forward.html), [live version](https://claude.ai/artifact/CnNTamggo7MCSHNrmoAAzh).
4. `encode_prompt` — `pipeline_stable_diffusion.py` L332–500 (Stage A)
5. Processor registry — `attention.py` L41–96 (Stage D)

## Task 2 — Turn it into a walkthrough repo, with GitHub Pages

Repo-hosting and structure layer only, no artifact content work (Task 1's `.html` files are the material; this just makes them walkable). Audience: a newcomer to Stable Diffusion, reading the repo top to bottom to learn it, not someone who already knows what they're looking for.

- Enable Pages on this repo (separate from the `rishavghosh605.github.io` portfolio repo, served at a sub-path, no conflict) so every page is viewable straight from a link, no download needed.
- One ordered folder per stage, numbered to match the X-Ray's own route order (the order a newcomer should actually read them in, not the order they were built in) — e.g. `01-latent-initialisation/`, `02-denoising-loop/`, `03-reverse-step/`, and so on through Task 1's five.
- Each folder's README: a short "what you'll learn here" before the link, plus the link itself (Artifact + Pages URL).
- A top-level index (this folder's `README.md`, extended) that walks a newcomer through the folders in order, like a syllabus, not just a file table.
