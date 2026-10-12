# 04 — DDIM's reverse step

**What you'll learn here:** what `self.scheduler.step(...)` actually computes, the call the denoising loop treats as a black box. For DDIM with `eta=0` (SD 1.5's deterministic case), it's a fixed, three-part computation: predict the clean image `x_0` in one shot from the current noisy latent, clip it, then recombine it with a "direction" term to get the next, less-noisy latent. No randomness, no further reverse steps hidden inside — the whole function is a closed-form transform of whatever noise prediction the UNet handed it.

You'll also see exactly where DDPM and DDIM diverge (same trained network, different update rule) and derive why `eta=1` exactly recovers DDPM's own stochastic step.

- **Artifact (interactive):** https://claude.ai/artifact/1r22ovvRUL1W1iwgMgL8Tb
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/ddim_reverse_step.html
- **Source file in this repo:** [`concepts/code-explorers/ddim_reverse_step.html`](../../concepts/code-explorers/ddim_reverse_step.html)
- **Pinned code:** `scheduling_ddim.py` lines 433–491, `DDIMScheduler.step` (diffusers v0.40.0)

Previous: [03 — The denoising loop](../03-denoising-loop/README.md). Next: [05 — The UNet forward pass](../05-unet-forward/README.md), opening the loop's other black box: the noise prediction itself.
