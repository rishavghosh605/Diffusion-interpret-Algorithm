# 03 — The denoising loop

**What you'll learn here:** the shape of the whole sampling process, Ho et al.'s Algorithm 2 written out as the actual 55-line Python loop that runs it. Each iteration does exactly two things: call the UNet to get a noise prediction, then call the scheduler to turn that prediction into the next, slightly-less-noisy latent. Classifier-free guidance's two embeddings from page 01 get used here for the first time, concatenated into one batch and split back apart after the UNet call; the latent page 02 drew is what this loop overwrites fifty times.

This is the skeleton the next four pages all hang off of. It treats both the UNet call and the scheduler's step as black boxes; pages 04 and 05 each open one of them.

- **Artifact (interactive):** https://claude.ai/artifact/HbbAryhY3tphNAEuicFct7
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/denoising_loop.html
- **Source file in this repo:** [`concepts/code-explorers/denoising_loop.html`](../../concepts/code-explorers/denoising_loop.html)
- **Pinned code:** `pipeline_stable_diffusion.py` lines 1028–1082, `StableDiffusionPipeline.__call__` (diffusers v0.40.0)

Previous: [02 — Latent initialisation](../02-latent-initialisation/README.md). Next: [04 — DDIM's reverse step](../04-reverse-step/README.md), opening the scheduler call this page treats as a black box.
