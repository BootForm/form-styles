# Layouts

How a form's fields are arranged on the page, independent of how any one field is styled (see
[inputs.md](inputs.md) for that).

## Stacked

The default, and the right choice unless there's a specific reason to use something else: one
field per row, label above the input. Fastest to scan, works at any width with no changes:

```html
<form action="https://f.bootform.com/{form_id}" method="POST" class="flex flex-col gap-4">
  <div class="flex flex-col gap-1">
    <label for="name" class="text-sm font-medium">Your name</label>
    <input id="name" name="name" type="text" required
           class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
  </div>
  <div class="flex flex-col gap-1">
    <label for="email" class="text-sm font-medium">Your email</label>
    <input id="email" name="email" type="email" required
           class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
  </div>
  <button type="submit" class="self-start rounded-md bg-brand-500 px-5 py-2 font-medium text-white hover:bg-brand-600">Send</button>
</form>
```

## Floating label

The label sits inside the field until it's focused or filled, then floats above it. Saves
vertical space on a short form; costs a little extra markup and a `peer`/`:placeholder-shown`
trick per field:

```html
<div class="relative">
  <input id="name" name="name" type="text" required placeholder=" "
         class="peer w-full rounded-md border border-gray-300 bg-white px-3 pb-2 pt-5 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
  <label for="name"
         class="pointer-events-none absolute left-3 top-4 text-gray-500 transition-all peer-focus:top-2 peer-focus:text-xs peer-focus:text-brand-500 peer-[:not(:placeholder-shown)]:top-2 peer-[:not(:placeholder-shown)]:text-xs dark:text-gray-400">
    Your name
  </label>
</div>
```

`placeholder=" "` (a single space, not an empty string) is required for `:placeholder-shown` to
work at all; an input with no `placeholder` attribute never matches that selector, floating or
not, in every browser this trick otherwise works in.

## Card

The whole form set inside an elevated, bordered container, separate from the page background;
what most of the BootForm VitePress templates' own contact pages use:

```html
<div class="rounded-xl border border-black/10 bg-white p-6 shadow-sm dark:border-white/10 dark:bg-white/5 sm:p-8">
  <form action="https://f.bootform.com/{form_id}" method="POST" class="flex flex-col gap-4">
    <!-- fields -->
  </form>
</div>
```

## Inline

A single-field form (a newsletter signup, most often) with the input and button on one row
instead of stacked; needs `sm:` breakpoints to fall back to stacked on a narrow screen rather than
squeezing both onto one line at 320px wide:

```html
<form action="https://f.bootform.com/{form_id}" method="POST" class="flex flex-col gap-2 sm:flex-row">
  <input name="email" type="email" required placeholder="you@example.com"
         class="flex-1 rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
  <button type="submit" class="rounded-md bg-brand-500 px-5 py-2 font-medium text-white hover:bg-brand-600">Subscribe</button>
</form>
```

## Two-column grid

Short fields (name, email) side by side, with anything longer (a message) spanning the full
width; saves vertical space on a form with several short fields without cramming them onto one
row each:

```html
<form action="https://f.bootform.com/{form_id}" method="POST" class="grid grid-cols-1 gap-4 sm:grid-cols-2">
  <div class="flex flex-col gap-1">
    <label for="name" class="text-sm font-medium">Your name</label>
    <input id="name" name="name" type="text" required
           class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
  </div>
  <div class="flex flex-col gap-1">
    <label for="email" class="text-sm font-medium">Your email</label>
    <input id="email" name="email" type="email" required
           class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5">
  </div>
  <div class="flex flex-col gap-1 sm:col-span-2">
    <label for="message" class="text-sm font-medium">Message</label>
    <textarea id="message" name="message" rows="4" required
              class="rounded-md border border-gray-300 bg-white px-3 py-2 outline-none focus:border-brand-500 focus:ring-2 focus:ring-brand-500/25 dark:border-gray-600 dark:bg-white/5"></textarea>
  </div>
  <button type="submit" class="self-start rounded-md bg-brand-500 px-5 py-2 font-medium text-white hover:bg-brand-600 sm:col-span-2">Send</button>
</form>
```
