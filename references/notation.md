# Notation

Fixed symbol table, taken from Fuest et al., *Diffusion Models and Representation
Learning: A Survey* (arXiv:2407.00783). Any math written elsewhere in this repo
should reuse these symbols rather than introducing new ones, so notes stay
directly comparable to the paper.

## Forward (noising) process

| Symbol | Meaning |
| --- | --- |
| `x_0 ~ p(x)` | a clean training sample, drawn from the data distribution |
| `x_1, ..., x_T` | progressively noised versions of `x_0`, produced by a Markov chain |
| `T` | total number of diffusion timesteps |
| `β_t` | time-dependent variance schedule at step `t` |
| `I` | identity matrix, dimensionality matching `x_0` |
| `p(x_t \| x_{t-1}) = N(x_t; sqrt(1-β_t) x_{t-1}, β_t I)` | one forward transition kernel (DDPM parametrization) |
| `α_t := 1 - β_t` | per-step signal retention |
| `ᾱ_t := ∏_{i=1}^{t} α_i` | cumulative signal retention up to step `t` |
| `p(x_t \| x_0) = N(x_t; sqrt(ᾱ_t) x_0, (1 - ᾱ_t) I)` | closed-form marginal, sampling `x_t` directly from `x_0` |
| `ε_t ~ N(0, I)` | the Gaussian noise vector added at step `t` |
| `x_t = sqrt(ᾱ_t) x_0 + sqrt(1 - ᾱ_t) ε_t` | reparametrization trick: `x_t` in one step, no need to iterate the chain |

## Reverse (denoising) process

| Symbol | Meaning |
| --- | --- |
| `x_T ~ π(x_T) = N(0, I)` | pure noise starting point for sampling |
| `p_θ(x_{t-1} \| x_t) := N(x_{t-1}; μ_θ(x_t, t), Σ_θ(x_t, t))` | learned reverse transition |
| `μ_θ(x_t, t)` | predicted mean of the reverse step, output by the denoising network |
| `Σ_θ(x_t, t)` | predicted (often fixed) covariance of the reverse step |
| `θ` | parameters of the denoising neural network |
| `ε_θ(x_t, t)` | network reparametrized to predict added noise instead of the mean directly |
| `x̂_0` | the network's estimate of the clean sample, recoverable from `x_t` and `ε_θ` |

## Training objective

| Symbol | Meaning |
| --- | --- |
| `L_vlb` | Variational Lower-Bound objective (used in place of intractable exact maximum likelihood) |
| `D_KL(· \|\| ·)` | Kullback-Leibler divergence |
| `L_simple = E_t E_{x_0} E_{ε_t} \|\| ε_t - ε_θ(x_t, t) \|\|^2` | simplified noise-prediction objective, derived from `L_vlb` |

## Continuous-time / SDE formulation

| Symbol | Meaning |
| --- | --- |
| `σ(t)` | continuous time-dependent noise level, replacing discrete `t` |
| `dx = f(x, t) dt + g(t) dw` | forward process as an Itô SDE |
| `f(·, t): R^d → R^d` | vector-valued drift coefficient |
| `g(·): R → R` | scalar-valued diffusion coefficient |
| `w` | standard Wiener process |
| `∇_x log p(x; σ(t))` | the score function |
| `D(x; σ)` | neural network approximating the score via L2-denoising |
| VP-SDE | Variance-Preserving SDE (Ho et al. DDPM parametrization, `f(x,t) = -½β(t)x`, `g(t) = sqrt(β(t))`) |
| VE-SDE | Variance-Exploding SDE (`f(x,t) = 0`, `g(t) = sqrt(2σ(t) dσ(t)/dt)`) |

## Conventions

- Bold lowercase (`x`, `ε`) denotes vectors; this table drops boldface for
  plain-text readability, but keep it in any rendered math (LaTeX/KaTeX).
  writing.
- Subscript `t` is always a timestep index, `θ` is always learned network
  parameters, and a hat (`x̂_0`) always denotes a network's estimate of a
  quantity rather than its ground truth.
- When a concept note needs a symbol not covered here, add it to this table
  in the same pull request rather than leaving it implicit.
