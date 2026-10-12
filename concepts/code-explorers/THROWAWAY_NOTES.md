# Throwaway notes — pending artifact fixes

Scratch list of real confusions/gaps caught while reading the pages, to be
batched into actual edits later. Not a permanent doc — clear entries out
once they're fixed in the artifact itself.

---

## unet_forward.html — sample vs encoder_hidden_states not clearly distinguished

**The confusion (verbatim from conversation):** reading the down_block call
signature (`hidden_states=sample, temb=emb, encoder_hidden_states=encoder_hidden_states`),
it wasn't clear that `sample` *is* the latents (x_t, shape (B,4,H/8,W/8)) and
`encoder_hidden_states` is CLIP's prompt_embeds (shape (B,77,768)) — i.e. two
unrelated tensors from two different sources (the VAE-latent side vs the
text-encoder side), not "the latents" and "some other form of the latents."
The encoder_hidden_states' CLIP-shaped dimensions (77, 768) got mistaken for
being related to the latent/prompt relationship in a way they aren't: it
never gets downsampled or reshaped by the UNet's own spatial blocks, it's
just handed unchanged into every cross-attention call as K/V source.

**Why this page didn't prevent it:** the Terms section defines `sample` and
mentions encoder_hidden_states only as "the prompt, processed," but never
states explicitly, in one place, "these are two different tensors from two
different sources, one is the image being denoised, one is fixed
conditioning, they are not two forms of the same thing." The distinction is
implied across several token flow descriptions but never stated as its own
plain sentence.

**Proposed fix:** add an explicit glossary entry (or expand gl-unetarch) on
unet_forward.html stating plainly: sample = x_t (image side, reshaped by
every block); encoder_hidden_states = prompt_embeds (text side, same tensor
handed unchanged to every cross-attention call, never downsampled). Possibly
also worth a one-line callout in cross_attention.html's own terms, since
that's where the actual K/V split happens.

**Status:** not yet applied. Batch with other pending fixes.
