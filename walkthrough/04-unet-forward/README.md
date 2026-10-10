# 04 — The UNet forward pass

**What you'll learn here:** the actual shape of `epsilon_theta(x_t, t)`, the noise-prediction network the whole algorithm is built around. Underneath one function symbol is a real U-shaped architecture: a convolution in, a chain of downsampling blocks, a bottleneck, a mirrored chain of upsampling blocks stitched back together with skip connections, a convolution out. This page skims the real method (257 lines, most of it ControlNet/T2I-Adapter/GLIGEN branches SD 1.5 never takes) down to the path that actually runs.

You'll trace exactly how the skip connections' bookkeeping lines each decoder block up with its matching encoder block, and open the sinusoidal timestep embedding that tells the network which denoising step it's on.

- **Artifact (interactive):** https://claude.ai/artifact/CnNTamggo7MCSHNrmoAAzh
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/unet_forward.html
- **Source file in this repo:** [`concepts/code-explorers/unet_forward.html`](../../concepts/code-explorers/unet_forward.html)
- **Pinned code:** `unet_2d_condition.py` lines 979–1235, `UNet2DConditionModel.forward` (diffusers v0.40.0)

Previous: [03 — DDIM's reverse step](../03-reverse-step/README.md). Next: [05 — Cross-attention](../05-cross-attention/README.md), opening the one mechanism inside every down, mid and up block that actually reads the prompt.
