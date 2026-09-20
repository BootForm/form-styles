# form-styles

**[Live site: bootform.github.io/form-styles](https://bootform.github.io/form-styles/)**

A live, browsable gallery of copy-paste Tailwind CSS for making a form look right: input types,
states, layouts, and full page sections. Every example renders on the page itself, next to the
exact syntax-highlighted code behind it, with a one-click copy button. There's also a light/dark
toggle in the nav, since half of what this repo covers is what changes in dark mode.

Backend-agnostic on purpose: every snippet is plain HTML, so it drops into a
[BootForm](https://bootform.com) form, any other form backend, or one that doesn't submit
anywhere yet.

If you're looking for how to wire a form's actual submission (the `action` URL, AJAX, file
uploads), that's [bootform.com/docs](https://bootform.com/docs) for React, Vue, Angular and
vanilla JS, and [`contact-form-recipes`](https://github.com/BootForm/contact-form-recipes) for
Astro, Hugo, Jekyll, Eleventy, Svelte and VitePress. This repo is styling only.

## What's in here

- **[Inputs](https://bootform.github.io/form-styles/inputs.html)** — date, number, phone, file
  upload, select, radio group, checkbox group.
- **[States](https://bootform.github.io/form-styles/states.html)** — focus, validation/error,
  success, submitting, and dark mode.
- **[Layouts](https://bootform.github.io/form-styles/layouts.html)** — stacked, floating label,
  card, inline, two-column grid.
- **[Sections](https://bootform.github.io/form-styles/sections.html)** — a form paired with
  contact info, an embedded map, an FAQ, or a newsletter band.

## How this is built

Plain HTML, no build step: Tailwind via `@tailwindcss/browser@4` (a real Tailwind v4 engine
running in-browser, not the old `cdn.tailwindcss.com` Play CDN, which only ever ships Tailwind
v3) and [Prism](https://prismjs.com/) for syntax highlighting. The copy button is hand-rolled with
`navigator.clipboard.writeText()` rather than Prism's own copy-to-clipboard plugin, which relies
on the deprecated `document.execCommand('copy')` and silently falls back to a "press Ctrl+C"
message in current Chrome instead of actually copying anything.

Each category page (`inputs.html`, `states.html`, `layouts.html`, `sections.html`) was generated
from the flat markdown this repo started as, via a one-off script, then hand-fixed and verified in
a real browser (see the fixes below). There's no ongoing dependency on that script; each page is
now a plain, hand-editable HTML file. Add a new example by copying an existing `<section>` block.

## Verified live before publishing

- `has-checked:` (used in the radio segmented-control example) is a Tailwind **v4-only**
  shorthand with no effect on v3; the arbitrary form `has-[:checked]:` works on both and is what
  every example here actually uses.
- `dark:[color-scheme:dark]` on a `type="date"` input fixes the native calendar icon/popup
  rendering black-on-black in dark mode; without it the picker ignores the page's own dark styling.
- Prism's `copy-to-clipboard` plugin doesn't work reliably in current Chrome (see above); replaced
  with a small hand-rolled button.
