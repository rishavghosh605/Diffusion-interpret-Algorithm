# 02 — Latent initialisation

**What you'll learn here:** how `prepare_latents` draws \(x_T\), literally line 1 of Ho et al.'s sampling algorithm. Twenty lines, no prompt read anywhere in them: `batch_size`, image size and a seed are all that decide the starting point. You'll derive the latent's spatial shape from the VAE's own downsampling (512×512 pixels becomes 4×64×64, a 48x reduction), and work out why `init_noise_sigma` exists at all — it turns out "pure noise" means something different depending on which scheduler's units you're in (PNDM's is 1.0, Euler's is about 14.61), and this function is where that difference actually gets applied.

The flowchart on this page is worth opening even if you skip the rest: it's the only place in the series that shows `prepare_latents`' own five internal steps nested inside the full `__call__` sequence, with the 50-iteration denoising loop drawn right after it.

- **Artifact (interactive):** https://claude.ai/artifact/PNQzNuxFspofLyXHHhFHWL
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/prepare_latents.html
- **Source file in this repo:** [`concepts/code-explorers/prepare_latents.html`](../../concepts/code-explorers/prepare_latents.html)
- **Pinned code:** `pipeline_stable_diffusion.py` lines 694–714, `StableDiffusionPipeline.prepare_latents` (diffusers v0.40.0)

Previous: [01 — Prompt encoding](../01-prompt-encoding/README.md). Next: [03 — The denoising loop](../03-denoising-loop/README.md), where this page's `x_T` becomes the loop variable that gets overwritten fifty times.
