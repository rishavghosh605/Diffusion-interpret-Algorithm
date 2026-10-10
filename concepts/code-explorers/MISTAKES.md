# Mistakes made building these pages

A running log of real mistakes caught while building this series, kept so the same one doesn't happen twice. Add an entry whenever a review (mine or Rishav's) catches something that should have been right the first time. Each entry: what went wrong, why it happened, the check that would have caught it, and whether that check is now automatic (see `VERIFICATION.md`) or still relies on noticing it by eye.

---

### Citing an equation number with no way to see the equation

**What happened:** `denoising_loop.html` cited "survey Eq. 13" seven times (glossary, notation table, four token math fields, the algorithm table) without ever showing what Eq. 13 actually says. A reader hits a bare citation with nothing to click. Fixing that one citation and then grepping the rest of the page turned up five more bare citations, to Eq. 1, 3, 5, 6 and 7, in the same shape — same mistake, just not yet flagged.

**Why:** Citations were written the way a paper cites its own equations — by number, trusting the reader has the paper open. This page's reader doesn't. One instance looked like a one-off until the grep showed it was the default way every equation got cited.

**Fix:** For each equation number, quote the actual equation (verified against the saved paper text, not recalled) inside the glossary entry for the thing it defines, then add a `GLP` auto-link pattern so every bare citation becomes clickable to it. Two citations sat outside any auto-linked selector (an eqref span in the algorithm table, a `where` field in the math-row sidebar) and needed a manual `.gl` span instead. One equation (survey Eq. 9, VP/VE) couldn't be pinned to an exact number — the extracted PDF text garbles that part of the numbering — so it got a softened, honest citation instead of a fabricated precise one.

**Now automatic?** Partly — promoted to `VERIFICATION.md` item 4 as a grep (every `Eq. N` resolves to a `.gl` span). Still relies on someone adding the glossary quote and pattern when a new equation gets cited in the first place; the grep only catches it after the fact.

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

**Now automatic?** Partly — `VERIFICATION.md` item 7 already says to use a local MathJax copy by default, which sidesteps the question. Still relies on recognizing a *new* blocked host for what it is, by eye, the first time.

---

### Line numbers mentioned everywhere, clickable nowhere

**What happened:** `denoising_loop.html` mentions specific code line numbers constantly — in the glossary, the derivations, the algorithm table, token math fields — but none of them were clickable. A reader following "line 1062" or "lines 1053 to 1055" had to scroll the code pane by hand every time, even though the page already knows exactly where every line is (each one has a `data-ln` attribute).

**Why:** The line numbers were written as plain prose citations, the same habit that caused the bare `Eq. N` citations. The infrastructure to jump to a line already existed in spirit (clicking a token already scrolls the code pane into view) but nothing extended that to a bare mention of a line number in running text.

**Fix:** Added a second text-scanning pass, `linkLines`, parallel to the existing glossary auto-linker (`linkTerms`) but matching `/\bline(s)?\s+\d{3,4}(...to...\d{3,4})?\b/i` instead of a fixed term list. Each number is checked against the page's own `CODE` array (`LN`, a set of the line numbers actually shown on this page) before being wrapped, so a reference to a line on a *different* page (like `prepare_latents`' lines 694 to 714) stays plain text instead of becoming a dead link. Clicking a wrapped number calls `goLine(n)`, which scrolls the code pane to that line and briefly flashes it (a new `.ln.flash` CSS class, independent of the existing token-highlight `.lit` class so the two don't fight).

**Now automatic?** Yes — it's part of the page's shared shell (CSS + two functions + one selector list), not page-specific data. Any future page built from this template inherits it for free, the same way the glossary auto-linker already does.

---

### A token wired into the code pane with no inspector entry

**What happened:** On `ddim_reverse_step.html`, the code pane marked `eta` as a clickable token (`[[eta|eta]]` on line 481) but `T` had no `eta` entry. Item 3 of `VERIFICATION.md` (cross-reference every ID) caught it before shipping: clicking that token would have opened an inspector panel for an id that didn't exist.

**Why:** `eta` is a function argument, same category as `use_clipped_model_output`, but it only appears once, inline in an arithmetic line (`std_dev_t = eta * variance ** (0.5)`), not in a `self.config.*` read the way the other arguments are. It read like plain code, not like a name worth its own token, so it didn't get added to `T` when the rest of the function's arguments did.

**Fix:** Added the missing `T.eta` entry (argument, default 0.0, flows into `std_dev_t`) and its `HOPS` entry. The cross-reference script is what caught it, not a reading pass; this is the exact failure mode item 3 exists for.

**Now automatic?** Yes — already `VERIFICATION.md` item 3, run as written. This entry just records a real instance of it firing.

---

### A Song et al. equation citation linking to the wrong glossary entry

**What happened:** While broadening the `GLP` pattern `/Song et al\.? Eq\. 1[26]/` on `ddim_reverse_step.html` to catch bare citations (it missed "Song et al. 2021 Eq. 16" and "Song et al.'s Eq. 12", both present on the page, because of the year and possessive in between), the first fix merged Eq. 12 and Eq. 16 into one pattern pointing at a single glossary id, `ddimupdate`. That's wrong for the Eq. 16 case: Eq. 16 is the variance formula (`self._get_variance`), Eq. 12 is the reverse-step update itself. A reader clicking the Eq. 16 citation would have landed on the wrong glossary entry.

**Why:** The two equation numbers share the same author citation, and widening a regex to catch more text naturally reaches for one pattern instead of two. The original narrow pattern's single destination carried over by default when the pattern was widened, without re-checking that both numbers it now caught actually meant the same thing.

**Fix:** Split into two patterns, one per equation number (`Eq\. 16` to `getvariance`, `Eq\. 12` to `ddimupdate`), and added the Eq. 16 citation to the `gl-getvariance` entry itself (it previously described the formula without citing it).

**Now automatic?** No. The cross-reference script confirms every `GLP` id resolves to a real `gl-` entry, but it can't tell whether that entry is the *right* one for the text matched — that's still a reading check, and it's why a fix to one mistake needs the same scrutiny as new content.
