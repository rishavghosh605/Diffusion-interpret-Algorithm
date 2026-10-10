# Mistakes made building these pages

A running log of real mistakes caught while building this series, kept so the same one doesn't happen twice. Add an entry whenever a review (mine or Rishav's) catches something that should have been right the first time. Each entry: what went wrong, why it happened, the check that would have caught it, and whether that check is now automatic (see `VERIFICATION.md`) or still relies on noticing it by eye.

---

### Citing an equation number with no way to see the equation

**What happened:** `denoising_loop.html` cited "survey Eq. 13" seven times (glossary, notation table, four token math fields, the algorithm table) without ever showing what Eq. 13 actually says. A reader hits a bare citation with nothing to click.

**Why:** Citations were written the way a paper cites its own equations — by number, trusting the reader has the paper open. This page's reader doesn't.

**Fix:** Quote the actual equation (verified against the saved paper text, not recalled) inside the glossary entry for the thing the equation defines, then add one `GLP` auto-link pattern (`/Eq\. 13/`) so every bare citation becomes clickable to it. Reuses existing infrastructure, no new UI.

**Now automatic?** No. Nothing currently greps for "Eq. N" and checks it resolves to something quotable. Worth a QA script eventually: every `Eq. \d+` on the page should be inside a `.gl` span or adjacent to one.

---

### A term explained on one page, assumed on another

**What happened:** `scale_model_input`'s math field on `denoising_loop.html` says "For VP schedulers... For VE schedulers..." without defining VP/VE anywhere on that page. The actual definition only exists on the `prepare_latents` page's glossary. Same thing for CLIP's `prompt_embeds` shape, 77 × 768 — mentioned on this page, but what 77 and 768 individually mean was only ever spelled out elsewhere.

**Why:** Each code explorer page is supposed to be self-contained (`code_explorer_plan.md` section 0: "no second page for the same function"), but when writing page N, terms already glossed on page N-1 felt "already covered" and didn't get re-checked against *this* page's own glossary.

**Fix:** Added `gl-vpve` to `denoising_loop.html`'s own glossary (not just a reference to the other page), and expanded the existing `gl-clip` entry to explain 77 and 768 individually instead of just stating the shape.

**Now automatic?** No, and hard to make fully automatic — it requires judgment about which terms are "used precisely enough to need their own entry" vs. "fine as prose." The cheap partial check: before shipping a page, read every token's `math` and `note` field once, and for every capitalized acronym or named quantity in them, confirm a `gl-` entry for it exists **on that same page**. This is the same move as the glossary-coverage check in `VERIFICATION.md` item 4 (every `GLP` pattern has an entry) — just aimed one level up, at terms that aren't in `GLP` yet because nobody added them.

---

## Standing check to add next time

Before calling a page done, in addition to `VERIFICATION.md`'s four scripted checks: read the full set of token `math`/`note`/`flow` text once, start to finish, as a reader who hasn't seen the other pages. Any named thing you'd have to ask about (an acronym, an equation number, a shape like "77 × 768") needs either a `gl-` entry on *this* page or to be fully explained inline. This is a reading pass, not yet a script — if it keeps catching things, it's worth scripting (flag every capitalized multi-letter token and every "Eq. N" and check each resolves to a `gl-` id).
