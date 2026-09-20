# form-styles

Copy-paste Tailwind CSS for making a form look right, not just work. Backend-agnostic on
purpose: every snippet is plain HTML, so it drops into a [BootForm](https://bootform.com) form,
any other form backend, or a form that doesn't submit anywhere yet.

If you're looking for how to wire a form's actual submission (the `action` URL, AJAX, file
uploads, a moderated feed), that's [bootform.com/docs](https://bootform.com/docs) for React, Vue,
Angular and vanilla JS, and [`contact-form-recipes`](https://github.com/BootForm/contact-form-recipes)
for Astro, Hugo, Jekyll, Eleventy, Svelte and VitePress. This repo is styling only.

## What's in here

- **[inputs](inputs.md)** — text, date, number, phone, file, select, radio group, checkbox group.
  The inputs a plain `<input type="...">` alone usually looks wrong for.
- **[states](states.md)** — focus, validation/error, success, and submitting states, plus what
  changes for dark mode.
- **[layouts](layouts.md)** — stacked, floating-label, card, inline, and two-column field
  arrangements.
- **[sections](sections.md)** — full page sections that pair a form with something else: contact
  details, an embedded map, an FAQ, a newsletter band. The thing you'd actually paste onto a
  contact page, not just a bare `<form>`.

## Before you copy a snippet

Every `<form>` below uses `action="https://f.bootform.com/{form_id}"` as a stand-in. Replace
`{form_id}` with your own (see `contact-form-recipes`'s README for how to get one and why never to
reuse an example ID), or swap the `action` and field names for whatever backend you're actually
using; the styling doesn't care which.

**If a class here doesn't seem to apply** (a border, background, or padding you added just isn't
showing up, especially on a VitePress site built from a BootForm template): several of Tailwind's
own base resets in those templates' compiled CSS end up unlayered while Tailwind's own utility
classes are layered, so a plain class can silently lose to the reset. Append `!` to the specific
class that needs to win (`border-gray-300!`, `px-3!`, `bg-brand-500!`). This is specific to that
one build setup, not a general Tailwind problem, so don't reach for it by default; see any
BootForm VitePress template's own `AGENTS.md` for the fuller explanation.
