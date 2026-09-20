# States

A form needs to look right at rest, and also while it's being used: focused, invalid, submitting,
and after it succeeds or fails. Most of this needs a little JavaScript to intercept the submit;
see [`contact-form-recipes`](https://github.com/BootForm/contact-form-recipes) for the fetch/AJAX
side of it per framework. This page is just the classes.

## Focus

Already part of the baseline in [inputs.md](inputs.md): `focus:border-brand-500 focus:ring-2
focus:ring-brand-500/25`. Worth calling out on its own, because it's the one state every field
needs and the one most often skipped: a field with no visible focus state fails basic
accessibility, and it's the first thing a keyboard user notices.

## Validation and error

Toggle `aria-invalid` on the field itself (screen readers announce it, and it's a real,
inspectable signal, unlike a CSS class alone) and show the error text right below the field:

```html
<div class="flex flex-col gap-1">
  <label for="email" class="text-sm font-medium">Your email</label>
  <input id="email" name="email" type="email" required aria-invalid="true" aria-describedby="email-error"
         class="rounded-md border border-red-400 bg-white px-3 py-2 outline-none focus:border-red-500 focus:ring-2 focus:ring-red-500/25 dark:border-red-500 dark:bg-white/5">
  <p id="email-error" class="text-sm text-red-600 dark:text-red-400">Enter a valid email address.</p>
</div>
```

Remove `aria-invalid`, its border/ring classes, and the `<p>` once the field becomes valid again;
don't leave a stale error showing after the visitor fixes it.

## Success

A field-level success state (a green border and a checkmark) for a field that's been validated
inline, separate from the form-level "your message was sent" state below:

```html
<div class="flex flex-col gap-1">
  <label for="email" class="text-sm font-medium">Your email</label>
  <div class="relative">
    <input id="email" name="email" type="email" required value="you@example.com"
           class="w-full rounded-md border border-green-400 bg-white px-3 py-2 pr-9 outline-none focus:border-green-500 focus:ring-2 focus:ring-green-500/25 dark:border-green-500 dark:bg-white/5">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="pointer-events-none absolute right-3 top-1/2 size-4 -translate-y-1/2 text-green-500"><path fill-rule="evenodd" d="M16.7 5.3a1 1 0 0 1 0 1.4l-8 8a1 1 0 0 1-1.4 0l-4-4a1 1 0 1 1 1.4-1.4L8 12.58l7.3-7.3a1 1 0 0 1 1.4 0Z" clip-rule="evenodd"/></svg>
  </div>
</div>
```

The whole-form success state, after a real submission, usually replaces the form entirely rather
than decorating it:

```html
<div class="rounded-lg border border-green-200 bg-green-50 p-6 text-center dark:border-green-500/30 dark:bg-green-500/10">
  <p class="font-medium text-green-800 dark:text-green-400">Thanks - your message was sent.</p>
  <p class="mt-1 text-sm text-green-700/80 dark:text-green-400/70">We usually reply within a day.</p>
</div>
```

## Submitting

Disable the button and swap its label, so a slow network doesn't invite a double-submit:

```html
<button type="submit" disabled
        class="inline-flex items-center gap-2 self-start rounded-md bg-brand-500 px-5 py-2 font-medium text-white opacity-70 disabled:cursor-not-allowed">
  <svg class="size-4 animate-spin" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none">
    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"/>
    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 0 1 8-8V0C5.37 0 0 5.37 0 12h4Z"/>
  </svg>
  Sending…
</button>
```

`disabled:cursor-not-allowed` only does anything while the `disabled` attribute is actually
present; toggle the attribute itself from the submit handler, not just a class.

## Dark mode

Every example above already carries its own `dark:` classes; the pattern to keep consistent
across a whole form:

- **Borders**: an explicit gray (`border-gray-300` / `dark:border-gray-600`), never an
  opacity-based color alone (`border-black/15`), which gets harder to see, not easier, on a dark
  background.
- **Backgrounds**: an explicit `bg-white` / `dark:bg-white/5`, not "no background," so the field
  doesn't blend into the page behind it in either mode.
- **Status colors** (red/green above): the `-400`/`-500` shade on a dark background instead of
  the same `-600`/`-700` shade used in light mode, which reads as too dark and low-contrast once
  the page itself is dark.
