# 05 — Cross-attention

**What you'll learn here:** the X-Ray's deepest stage, and the one function behind both self-attention and cross-attention in every transformer block of the UNet. The two are the exact same code: query always comes from the image's own features; key and value come from whichever tensor is `encoder_hidden_states` at call time — the image itself for self-attention, the prompt for cross-attention. Nothing else differs.

You'll trace real tensor shapes through the attention matrix (spatial tokens × prompt tokens, literally one row per image position, one column per word of the prompt) and derive the scaled dot-product formula from the code's own `torch.baddbmm` call.

- **Artifact (interactive):** https://claude.ai/artifact/TbWNMtDZ7QMh3ZNi7SKzju
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/cross_attention.html
- **Source file in this repo:** [`concepts/code-explorers/cross_attention.html`](../../concepts/code-explorers/cross_attention.html)
- **Pinned code:** `attention_processor.py` lines 1141–1162, `AttnProcessor.__call__` (diffusers v0.40.0)

Previous: [04 — The UNet forward pass](../04-unet-forward/README.md). Next: [06 — The processor registry](../06-processor-registry/README.md), the mechanism for replacing this function across the whole UNet at once.
