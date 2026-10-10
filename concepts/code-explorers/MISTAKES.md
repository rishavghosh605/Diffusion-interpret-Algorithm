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

**Now automatic?** Partly — promoted to `VERIFICATION.md` item 5 as a required reading pass. Still judgment, not a script: it requires deciding which terms are "used precisely enough to need their own entry" vs. "fine as prose."

---

### Re-explaining a term inline instead of linking to the glossary

**What happened:** Early token `math`/`note` fields, when they touched a term worth real explanation (a named method, a contrast with another method, historical context), wrote the explanation out inline, in the inspector box itself. The boxes started carrying paragraph-length asides that crowded out the thing the box actually exists for (what this name looks like, where it comes from and goes to).

**Why:** Writing the explanation right where the question comes up feels more helpful in the moment than it is on the page as a whole — it works once, then makes every other box noisier for readers who already know the term.

**Fix:** Name the term inline, in plain text, exactly as it appears in the glossary's `<dt>`. Put the actual explanation, including any nuance, in the glossary entry. The page's existing `GLP`/`.gl` auto-link mechanism turns the matching phrase into a click-through automatically, as long as it's inside a linked selector (`.panel .f p`, `.panel .note`, `.panel .kind`, etc. — already wired up in every page's script). No manual link markup needed. This is also just `code_explorer_plan.md`'s own rule ("the glossary on the page is the explanation," external links are out) applied one level down, to the page's own inline boxes and not just to outside sources.

**Now automatic?** No. It's a writing-time judgment call (does this box need its own paragraph, or does the term already have — or deserve — a glossary entry). Reviewed by eye.

---

### Treating the sandbox's blocked network as a page bug

**What happened:** QA against jsdelivr (MathJax's CDN) failed inside this environment's sandbox (its network policy blocks the domain), and the failure briefly looked like it might be the page's fault before the cause was confirmed.

**Why:** A blocked-CDN console error and a real MathJax bug look identical from the page's own console output; the difference is environmental, not visible on the page.

**Fix:** When QA fails on a network call, check whether the *sandbox* can reach that host at all before assuming the page is wrong (`curl -sS "$HTTPS_PROXY/__agentproxy/status"`, or just try the same host for something unrelated). If it's a sandbox policy block, swap in a local copy for the sandbox test only (`npm install mathjax@3.2.2`, point the test's `<script src>` at it) and leave the real CDN link in the shipped page — the sandbox's restrictions aren't the reader's.

**Now automatic?** Partly — `VERIFICATION.md` item 6 already says to use a local MathJax copy by default, which sidesteps the question. Still relies on recognizing a *new* blocked host for what it is, by eye, the first time.
