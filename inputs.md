# Inputs

The baseline every other input on this page starts from: an explicit border color (not an
opacity-based one like `border-black/15`, which reads as invisible against most backgrounds), a
visible focus ring, and an explicit background so dark mode doesn't leave the field blending into
the page behind it.

```html
<div class="flex flex-col gap-1">
  <label for="name" class="text-sm font-medium">Your name</label>
  <input id="name" name="name" type="text" required
         class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
</div>
```

Swap `brand-500`/`brand-600` for whatever primary color the site actually defines; every example
below assumes those two utilities exist.

## Date

A native `<input type="date">` picks up this exact same styling; no JavaScript date-picker library
needed unless the design calls for a range picker or a non-native calendar UI:

```html
<div class="flex flex-col gap-1">
  <label for="move-in" class="text-sm font-medium">Preferred date</label>
  <input id="move-in" name="move_in_date" type="date" required
         class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5 dark:[color-scheme:dark]">
</div>
```

`dark:[color-scheme:dark]` fixes the native calendar icon and popup rendering as black-on-black in
dark mode; without it, the picker itself still uses light-mode colors even though the field around
it doesn't.

## Number

A quantity or count field. Same base styling, plus `min`/`max`/`step` for real validation instead
of just a visual hint:

```html
<div class="flex flex-col gap-1">
  <label for="guests" class="text-sm font-medium">Number of guests</label>
  <input id="guests" name="guests" type="number" min="1" max="20" step="1" required
         class="w-24 rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
</div>
```

`w-24` instead of a full-width field: a number this short doesn't need the same width as an email
field, and a full-width number input reads oddly.

## Phone

A `tel` input, not `text`, gets phone-appropriate mobile keyboards for free and lets `pattern`
validate a rough shape without a JavaScript library:

```html
<div class="flex flex-col gap-1">
  <label for="phone" class="text-sm font-medium">Phone number</label>
  <input id="phone" name="phone" type="tel" autocomplete="tel"
         pattern="[0-9+()\-\s]{7,}" placeholder="+1 (555) 555-0123"
         class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
</div>
```

A separate country-code `<select>` next to this field is a common upgrade; skip it unless the
site actually serves multiple countries; it's one more required decision for most visitors.

## File upload

Native `<input type="file">` is nearly impossible to style directly across browsers; the reliable
pattern is hiding it and styling a `<label>` that triggers it instead:

```html
<div class="flex flex-col gap-1">
  <span class="text-sm font-medium">Resume</span>
  <label for="resume" class="flex w-fit cursor-pointer items-center gap-2 rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium hover:bg-gray-50 dark:border-gray-600 dark:bg-white/5 dark:hover:bg-white/10">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="size-4"><path d="M9.25 13.25a.75.75 0 0 0 1.5 0V4.66l2.1 2.1a.75.75 0 1 0 1.06-1.06l-3.38-3.38a.75.75 0 0 0-1.06 0L6.09 5.7a.75.75 0 0 0 1.06 1.06l2.1-2.1v8.59Z"/><path d="M3.5 12.75a.75.75 0 0 0-1.5 0v2.5A2.75 2.75 0 0 0 4.75 18h10.5A2.75 2.75 0 0 0 18 15.25v-2.5a.75.75 0 0 0-1.5 0v2.5c0 .69-.56 1.25-1.25 1.25H4.75c-.69 0-1.25-.56-1.25-1.25v-2.5Z"/></svg>
    <span id="resume-filename">Choose a file</span>
  </label>
  <input id="resume" name="resume" type="file" required class="sr-only">
</div>

<script>
  document.getElementById('resume').addEventListener('change', (event) => {
    document.getElementById('resume-filename').textContent = event.target.files[0]?.name ?? 'Choose a file'
  })
</script>
```

`sr-only` (a standard Tailwind utility: visually hidden, still in the accessibility tree and still
focusable/keyboard-operable) rather than `hidden`, which would also remove it from tab order and
break keyboard users entirely.

## Select

```html
<div class="flex flex-col gap-1">
  <label for="budget" class="text-sm font-medium">Budget range</label>
  <select id="budget" name="budget" required
          class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
    <option value="" disabled selected>Choose one</option>
    <option value="under-1k">Under $1,000</option>
    <option value="1k-5k">$1,000 - $5,000</option>
    <option value="5k-plus">$5,000+</option>
  </select>
</div>
```

## Radio group (segmented control look)

A styled radio group reads as a set of buttons instead of the browser's default small circles,
using `peer` and `:has()` so no JavaScript is needed to toggle the selected look:

```html
<fieldset class="flex flex-col gap-1">
  <legend class="text-sm font-medium">Project type</legend>
  <div class="flex gap-2">
    <label class="has-[:checked]:border-brand-500 has-[:checked]:bg-brand-50 has-[:checked]:text-brand-600 dark:has-[:checked]:bg-brand-500/20 dark:has-[:checked]:text-brand-500 flex-1 cursor-pointer rounded-md border border-gray-300 px-4 py-2 text-center text-sm font-medium dark:border-gray-600">
      <input type="radio" name="project_type" value="new" class="sr-only" checked>
      New site
    </label>
    <label class="has-[:checked]:border-brand-500 has-[:checked]:bg-brand-50 has-[:checked]:text-brand-600 dark:has-[:checked]:bg-brand-500/20 dark:has-[:checked]:text-brand-500 flex-1 cursor-pointer rounded-md border border-gray-300 px-4 py-2 text-center text-sm font-medium dark:border-gray-600">
      <input type="radio" name="project_type" value="redesign" class="sr-only">
      Redesign
    </label>
  </div>
</fieldset>
```

`has-[:checked]:` needs Tailwind v3.4+ (the `:has()` variant); on an older Tailwind, style the
sibling `<input>`'s `checked` state with `peer-checked:` on an adjacent element instead.

## Checkbox group

```html
<fieldset class="flex flex-col gap-2">
  <legend class="text-sm font-medium">What do you need?</legend>
  <label class="flex items-center gap-2 text-sm">
    <input type="checkbox" name="needs[]" value="design" class="size-4 rounded border-gray-300 text-brand-500 focus:ring-brand-500/25 dark:border-gray-600">
    Design
  </label>
  <label class="flex items-center gap-2 text-sm">
    <input type="checkbox" name="needs[]" value="development" class="size-4 rounded border-gray-300 text-brand-500 focus:ring-brand-500/25 dark:border-gray-600">
    Development
  </label>
</fieldset>
```

`text-brand-500` on a checkbox/radio input sets its own check/dot color, since browsers render
native checkbox and radio marks using `currentColor`/`accent-color` under the hood; Tailwind's
`text-*` utilities are what set that.
