# 07 — The processor registry

**What you'll learn here:** how to find, and replace, every attention layer's processor in a UNet at once. Two small recursive methods, inherited for free by every UNet instance, walk the entire module tree once to collect a dict of every attention layer's dotted name and current processor, and a second time to install a new one (either the same instance everywhere, or a different one per layer via a dict keyed by those exact dotted names).

This is the finest-grained intervention point named anywhere in this series: a custom processor's own `__call__` (same shape as page 06's `AttnProcessor.__call__`) runs at every single attention call, dozens of times per sampling step — not just once per step, the way `callback_on_step_end` does.

- **Artifact (interactive):** https://claude.ai/artifact/VQ9xGBsEg23CXrcM119KBB
- **Pages (same page, no Claude account needed):** https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/processor_registry.html
- **Source file in this repo:** [`concepts/code-explorers/processor_registry.html`](../../concepts/code-explorers/processor_registry.html)
- **Pinned code:** `attention.py` lines 41–96, `AttentionMixin.attn_processors` / `set_attn_processor` (diffusers v0.40.0)

Previous: [06 — Cross-attention](../06-cross-attention/README.md). That's the whole walkthrough — see the [top-level index](../../README.md) for where to go from here.
