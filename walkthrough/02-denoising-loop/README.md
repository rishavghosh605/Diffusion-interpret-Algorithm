# 02 — The denoising loop

**What you'll learn here:** the shape of the whole sampling process, Ho et al.'s Algorithm 2 written out as the actual 55-line Python loop that runs it. Each iteration does exactly two things: call the UNet to get a noise prediction, then call the scheduler to turn that prediction into the next, slightly-less-noisy latent. Classifier-free guidance's two embeddings from the previous page get used here for the first time, concatenated into one batch and split back apart after the UNet call.

This is the skeleton the next four pages all hang off of. It treats both the UNet call and the scheduler's step as black boxes; pages 03 and 04 each open one of them.

- **Artifact (interactive):** https://claude.ai/artifact/HbbAryhY3tphNAEuicFct7
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/denoising_loop.html
- **Source file in this repo:** [`concepts/code-explorers/denoising_loop.html`](../../concepts/code-explorers/denoising_loop.html)
- **Pinned code:** `pipeline_stable_diffusion.py` lines 1028–1082, `StableDiffusionPipeline.__call__` (diffusers v0.40.0)

Previous: [01 — Prompt encoding](../01-prompt-encoding/README.md). Next: [03 — DDIM's reverse step](../03-reverse-step/README.md), opening the scheduler call this page treats as a black box.
