# Working in this repo

Plain HTML, no build step, no `package.json`. `index.html`, `inputs.html`, `states.html`,
`layouts.html`, `sections.html`: five files, each self-contained. GitHub Pages serves this repo
directly from `main` (see `.nojekyll`, needed to stop GitHub Pages running this through Jekyll,
which plain-HTML repos need but a VitePress-based repo in this org doesn't).

## Adding a new example

Every category page follows the same `<section>` shape: a heading, a short explanation, a live
rendered demo (real HTML, not a screenshot), and the same HTML again inside a
`<div class="code-block relative">` wrapper with a `.copy-btn` button. Copy an existing
`<section>` block as the starting point rather than writing one from scratch; the class names on
the wrapper divs (`not-prose`, `code-block`, `copy-btn`) are load-bearing for the copy button and
Prism's highlighting to find the right element, not just visual utility classes.

## The copy button is hand-rolled, not Prism's own plugin

Prism ships a `copy-to-clipboard` plugin, and it doesn't work reliably in current Chrome: it uses
the deprecated `document.execCommand('copy')`, which Chrome increasingly refuses, silently
falling back to a "Press Ctrl+C to copy" message with nothing actually on the clipboard. This
repo uses a small hand-rolled button instead (`navigator.clipboard.writeText()`, see the
`<script>` near the end of each page). Confirmed both ways in a real browser before choosing this:
the execCommand approach visibly failed, the Clipboard API approach worked.

## `has-checked:` vs `has-[:checked]:`

`has-checked:` (and the other `has-*` shorthand variants) is a **Tailwind v4-only** convenience
alias with zero effect on Tailwind v3 (confirmed: generates no CSS rule at all, no error either).
The arbitrary-variant form, `has-[:checked]:`, works on both v3.4+ and v4 and is what every
example on this site actually uses (the radio segmented-control pattern on the Inputs page).
Prefer the arbitrary form in anything meant to work regardless of which Tailwind version a reader
has, since a reader copying this into their own project has no guarantee which version they're on.

## Native form controls with their own popup (select, date) need explicit `color-scheme`

A native `<select>`'s dropdown list and a native `<input type="date">`'s calendar popup are
rendered by the browser itself, not by our CSS, and by default they follow the browser/OS's own
light-or-dark preference, not the page's own toggle. **Hit in practice 2026-09-20**: reported as
"the select shows all white until I hover" in both themes, since the popup was defaulting to
whatever the browser/OS preferred regardless of this page's own light/dark state. Fix: set
`[color-scheme:light]` and `dark:[color-scheme:dark]` directly on the control (see the Select and
Date examples on the Inputs page). This gives the browser explicit permission to pick the palette
that actually matches the page instead of guessing from an unrelated system setting.

**`color-scheme` alone was still reported broken for the select popup after that fix** ("still all
white in dark mode"), and turned out to be exactly that: real, still needed, but not sufficient by
itself in the browser tested. The reliable second half: set `background-color`/`color` directly on
each `<option>` (`dark:bg-ink dark:text-paper` alongside the light-mode equivalents) — Chrome
specifically honors per-`<option>` background/text color for the popup list, even though it
ignores almost every other CSS property on `<option>`. Keep both fixes together; `color-scheme`
alone is a plausible-looking fix that doesn't fully hold up.

Any other
native-popup control added later (`<input type="week">`, `<input type="time">`, a native color
picker) will need the same treatment.

## Dark mode toggle

`@custom-variant dark (&:where(.dark, .dark *));` plus a `#theme-toggle` button that flips
`.dark` on `<html>` and remembers the choice in `localStorage`, read back before paint (a plain
`<script>` tag before the Tailwind style block, not inside it) so the page doesn't flash light
mode for a moment on reload. Every demo box and code sample already has real `dark:` classes; the
toggle exists so a visitor can actually see them without opening devtools, which is the whole
point of a page that documents dark mode as one of its own sections.

## Writing style

- Second person, present tense, short sentences.
- **No em dashes or en dashes.** Use a comma, a colon, a full stop, or brackets.
- No AI attribution in commits or pull requests, here or anywhere else in this organisation.
