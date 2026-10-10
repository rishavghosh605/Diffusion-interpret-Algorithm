# 01 — Prompt encoding

**What you'll learn here:** how a plain text prompt becomes the tensor the rest of the pipeline actually reads. CLIP's tokenizer pads or truncates every prompt to exactly 77 tokens; CLIP's text encoder turns those tokens into a `(1, 77, 768)` tensor called `prompt_embeds`. You'll also see the thing most explanations skip: classifier-free guidance needs a *second* full encoding pass, of the empty string by default, not a zeroed-out vector — and why that's the only architecturally sound choice.

This runs once, before the denoising loop starts. Everything downstream (cross-attention, specifically) only ever sees the tensor this page produces, never the original string.

- **Artifact (interactive):** https://claude.ai/artifact/1D5aAZmqgfV1NsAxp9aUX6
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/encode_prompt.html
- **Source file in this repo:** [`concepts/code-explorers/encode_prompt.html`](../../concepts/code-explorers/encode_prompt.html)
- **Pinned code:** `pipeline_stable_diffusion.py` lines 332–512, `StableDiffusionPipeline.encode_prompt` (diffusers v0.40.0)

Next: [02 — The denoising loop](../02-denoising-loop/README.md), where `prompt_embeds` gets used for the first time.
