# Diffusion Interpret Algorithm

A working notebook for understanding and interpreting diffusion model concepts, one piece at a time. Each concept gets its own space: a plain-language explanation, the math or code behind it, and notes on why it works the way it does.

This is nascent by design. It grows as understanding grows, not all at once.

## Structure

- `concepts/` — one folder per diffusion concept (e.g. forward process, denoising, score matching, schedulers, classifier-free guidance). Each folder holds a `README.md` (definition + intuition) and code/notebooks where relevant.
- `walkthrough/` — a newcomer's path through Stable Diffusion 1.5's actual source code, in reading order. See [Learn SD 1.5 from its own code](#learn-sd-15-from-its-own-code) below.
- `notebooks/` — exploratory or experimental notebooks that don't belong to a single finalized concept yet.
- `design-system/` — reference copy of the personal design language used for any artifacts, diagrams, or visual explainers built from this repo. See `design-system/README.md`.
- `references/` — source papers this repo is built from, plus `notation.md`, the fixed notation convention all math in this repo follows. See `references/README.md`.

## How this repo is used

Concepts move through three stages:

1. **Raw** — a rough note or question, captured with minimal friction.
2. **In progress** — being worked through with code, math, or a diagram.
3. **Finalized** — a clear definition, working code, and correct information, ready to reference later.

Only finalized material should read as settled truth. Everything else is marked as still forming.

## Learn SD 1.5 from its own code

Six self-contained pages, each one real function from HuggingFace `diffusers` v0.40.0, pinned to exact line numbers and verified character-for-character against the actual source. No step is taken on faith: every claim on every page cites the line that proves it, and every page names what it's skipping, not just what it's showing.

Read them in this order. Each one opens a black box the previous page left closed.

1. **[Prompt encoding](walkthrough/01-prompt-encoding/README.md)** — a string becomes `prompt_embeds`, the tensor everything downstream actually reads.
2. **[The denoising loop](walkthrough/02-denoising-loop/README.md)** — the whole sampling algorithm, as the 55 lines of Python that run it.
3. **[DDIM's reverse step](walkthrough/03-reverse-step/README.md)** — what `scheduler.step(...)` computes: a fixed, deterministic transform, no hidden randomness.
4. **[The UNet forward pass](walkthrough/04-unet-forward/README.md)** — the real U-shaped architecture behind `epsilon_theta(x_t, t)`, skimmed to SD 1.5's actual execution path.
5. **[Cross-attention](walkthrough/05-cross-attention/README.md)** — the one function behind both self- and cross-attention, and how the prompt actually reaches the image.
6. **[The processor registry](walkthrough/06-processor-registry/README.md)** — how to find and replace every attention layer's processor in the whole UNet at once, the finest-grained intervention point in the series.

Not yet built: **latent initialization** (`prepare_latents`, Algorithm 2's first row, \(x_T\sim\mathcal N(0,I)\)) is referenced from several of the pages above as "a separate page," but doesn't exist yet. The walkthrough currently starts at prompt encoding instead.

Each page is a self-contained HTML file: open it directly (locally, or via GitHub Pages once enabled — see below) or as a live Claude Artifact, no setup required either way. The build process, verification checklist, and a running log of real mistakes caught along the way live in [`concepts/code-explorers/`](concepts/code-explorers/README.md).

### Viewing these pages without downloading anything

Every page's Pages link above assumes GitHub Pages is enabled for this repo, served from this branch's root. If a Pages link 404s, Pages isn't turned on yet: go to **Settings → Pages** on this repository, set **Source** to "Deploy from a branch," pick this branch and `/ (root)`, and save — the pages will be live at `https://rishavghosh605.github.io/Diffusion-interpret-Algorithm/concepts/code-explorers/<page>.html` within a minute or two. (This repo's Pages site is separate from the `rishavghosh605.github.io` portfolio repo's own site; they don't conflict.) Until then, the Artifact links work immediately, and every `.html` file can be opened directly from a local clone.
