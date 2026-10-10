# Verifying a code explorer page: what's actually worth doing

Written after building `denoising_loop.html`. The full QA checklist in `code_explorer_plan.md` (section 10) asks for a Playwright sweep across every token, every viewport, both color schemes. In practice that step took longer than writing the page and mostly re-confirmed what three cheap script checks had already shown. This is the lean version: same confidence, a few minutes instead of a long interactive run.

## Do these four, always (seconds, scripted, no browser)

Plus one reading pass, below — not yet scripted.

1. **Syntax check.** Extract the `<script>` block and run `node --check` on it. Catches typos that would otherwise fail silently in the browser.
2. **Line-for-line diff against the real source.** Strip the `[[id|text]]`/`<k>`/`<s>`/`<c>` markers back to plain text and diff every `CODE` line against the actual pinned file (`pip download <pkg>==<version>` + `sed -n`, or the equivalent for the language). This is the check that matters most: it's the only thing that verifies the page's central claim, that the code shown is the code that runs. A handful of lines of Python does this in one shot.
3. **Cross-reference every ID.** Script it: every id used in `CODE` exists in `T`; every id in `T` is used in `CODE`; every `ROWS[k].tok` and every `SYM2TOK` value points at a real `T` id; every `T` id has a `HOPS` entry. Orphaned or missing ids are the most common copy-paste mistake when building these pages, and this check finds all of them at once.
4. **Grep-level house rules.** No em dash (`—`), no `<a href>` outside the header's two links, every `GLP` pattern has a matching `gl-` entry. Each is one regex.

5. **Every term is defined on *this* page.** Read every token's `math`/`note`/`flow` text once, start to finish. Any acronym, equation number, or shape (like "77 × 768") needs a `gl-` entry *on this page* — not just on another page in the series — or a full inline explanation. Each page is supposed to be self-contained; see `MISTAKES.md` for what happens when a term leans on another page's glossary.

If these five pass, the page's content is correct, internally consistent, and self-contained. That's the part that's expensive to get wrong and cheap to check.

## Do this once, briefly (a minute or two, browser, no interaction loop)

6. **Two or three screenshots, not a click sweep.** Open the page with a local MathJax copy if the CDN is blocked in-sandbox (`npm install mathjax@3.2.2`, swap the `<script src>`), force `data-theme="light"` so the screenshot doesn't depend on server time, and take:
   - the page as loaded (checks MathJax typeset, layout, code coloring all at once)
   - one token clicked, to confirm the inspector panel updates
   - the flowchart opened and one box expanded

   This catches the failure modes that a text-level diff can't see (a stray unclosed tag breaking layout, MathJax silently failing to load, the dialog not opening) without scripting every interaction.

## Skip, unless something specific is in doubt

- Clicking every token, every math symbol, every flowchart box, at every viewport, in both color schemes. If checks 1 to 5 pass and the screenshots look right, this doesn't turn up anything new — it just spends a lot of wall-clock time re-verifying the same rendering path over and over with different data.
- A full mobile-width interaction pass. Worth doing only if the page has mobile-specific layout logic being changed (the bottom-sheet inspector, the loopback arrow hiding) — not by default.
- Assuming a network failure during QA is a page bug without checking whether the sandbox itself can reach that host — see `MISTAKES.md`.

## The rule of thumb

Script what's checking a *fact* (does this line match the source, does this id exist). Screenshot what's checking a *rendering*. Don't use a browser automation loop to re-check a fact you could have diffed, and don't skip the one or two screenshots that are the only way to catch a rendering break.
