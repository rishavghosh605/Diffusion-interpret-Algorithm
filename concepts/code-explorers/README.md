# Code explorers

Self-contained HTML pages that explain one function of a real codebase: what every name in it is, where each value comes from and goes to, which function calls it and why, and which part of the underlying theory it implements, in fixed notation (see `references/notation.md`).

Built per `code_explorer_plan.md`'s format (kept privately, not in this repo): the reference implementation is `StableDiffusionPipeline` from `diffusers`, pinned to v0.40.0.

Open a page directly in a browser, or view it live as a Claude Artifact.

| Page | Function | Source (pinned) | Artifact |
| --- | --- | --- | --- |
| `denoising_loop.html` | The denoising loop, `StableDiffusionPipeline.__call__` step 7 | [`pipeline_stable_diffusion.py` L1028-L1082](https://github.com/huggingface/diffusers/blob/v0.40.0/src/diffusers/pipelines/stable_diffusion/pipeline_stable_diffusion.py#L1028-L1082) | [live version](https://claude.ai/artifact/HbbAryhY3tphNAEuicFct7) |

Each page is self-contained (inline CSS/JS, MathJax from a CDN) and follows Rishav's design language (see `design-system/`).

## Standing convention: inline boxes stay short, the glossary holds the explanation

When a token's or math symbol's inspector box ("Looks like" / "In the math" / "Comes from, goes to") touches a term that deserves real explanation (a named method, a contrast with another method, historical context), don't write the explanation inline. Instead:

1. Name the term in the inline box, in plain text, exactly as it appears in the glossary's `<dt>` (e.g. "classifier-free guidance").
2. Put the actual explanation, including any nuance (what it's contrasted with, why it exists, the paper it's from), in the glossary entry itself.
3. The page's existing `GLP`/`.gl` auto-link mechanism turns the matching phrase into a click-through to the glossary automatically — no manual link markup needed, as long as the phrase is inside a linked selector (`.panel .f p`, `.panel .note`, `.panel .kind`, etc., already wired up in every page's script).

This keeps the inspector boxes scannable while still letting a reader go one click deep on any term they don't already know, consistent with `code_explorer_plan.md`'s own rule that "the glossary on the page is the explanation" and external links are out.

