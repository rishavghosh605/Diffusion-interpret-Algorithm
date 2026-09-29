# Code explorers

Self-contained HTML pages that explain one function of a real codebase: what every name in it is, where each value comes from and goes to, which function calls it and why, and which part of the underlying theory it implements, in fixed notation (see `references/notation.md`).

Built per `code_explorer_plan.md`'s format (kept privately, not in this repo): the reference implementation is `StableDiffusionPipeline` from `diffusers`, pinned to v0.40.0.

Open a page directly in a browser, or view it live as a Claude Artifact.

| Page | Function | Source (pinned) | Artifact |
| --- | --- | --- | --- |
| `denoising_loop.html` | The denoising loop, `StableDiffusionPipeline.__call__` step 7 | [`pipeline_stable_diffusion.py` L1028-L1082](https://github.com/huggingface/diffusers/blob/v0.40.0/src/diffusers/pipelines/stable_diffusion/pipeline_stable_diffusion.py#L1028-L1082) | [live version](https://claude.ai/artifact/HbbAryhY3tphNAEuicFct7) |

Each page is self-contained (inline CSS/JS, MathJax from a CDN) and follows Rishav's design language (see `design-system/`).
