# Diffusion Interpret Algorithm

A working notebook for understanding and interpreting diffusion model concepts, one piece at a time. Each concept gets its own space: a plain-language explanation, the math or code behind it, and notes on why it works the way it does.

This is nascent by design. It grows as understanding grows, not all at once.

## Structure

- `concepts/` — one folder per diffusion concept (e.g. forward process, denoising, score matching, schedulers, classifier-free guidance). Each folder holds a `README.md` (definition + intuition) and code/notebooks where relevant.
- `notebooks/` — exploratory or experimental notebooks that don't belong to a single finalized concept yet.
- `design-system/` — reference copy of the personal design language used for any artifacts, diagrams, or visual explainers built from this repo. See `design-system/README.md`.

## How this repo is used

Concepts move through three stages:

1. **Raw** — a rough note or question, captured with minimal friction.
2. **In progress** — being worked through with code, math, or a diagram.
3. **Finalized** — a clear definition, working code, and correct information, ready to reference later.

Only finalized material should read as settled truth. Everything else is marked as still forming.
