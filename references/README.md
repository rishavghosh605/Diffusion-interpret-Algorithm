# References

Papers this repo's concepts and notation are built from.

## Primary source for notation

**Fuest, M., Ma, P., Gui, M., Schusterbauer, J., Hu, V. T., & Ommer, B. (2024).**
*Diffusion Models and Representation Learning: A Survey.* arXiv:2407.00783.
https://arxiv.org/abs/2407.00783

- Full text: [`omar-2024-diffusion-survey.md`](./omar-2024-diffusion-survey.md)
- Notation cheat sheet, kept in sync with this paper: [`notation.md`](./notation.md)

**Note on the file:** this environment's network policy blocks direct access to
arxiv.org (and its mirrors), so the exact PDF couldn't be downloaded into the repo.
The full paper text was retrieved instead and saved as Markdown, preserving every
section, equation, and reference. If a literal PDF copy is needed later, fetch it
from https://arxiv.org/pdf/2407.00783 from an unrestricted network and drop it in
this folder as `omar-2024-diffusion-survey.pdf`.

## Notation convention

Whenever notation or mathematical formulations of diffusion concepts are needed
in this repo, they default to the notation used in this survey (rather than, say,
the original DDPM or score-matching papers), even when other papers use different
symbols for the same idea. See `notation.md` for the fixed symbol table.
