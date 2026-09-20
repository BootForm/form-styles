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
