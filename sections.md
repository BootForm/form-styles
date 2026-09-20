# Sections

A bare `<form>` dropped onto a page is rarely the whole contact section a real site wants. These
pair a form with something else, the way a contact page usually needs to. Each one assumes the
[card layout](layouts.md#card) and the [standard field styling](inputs.md) from elsewhere in this
repo; only the section-level arrangement is new here.

## Form + contact info

Contact details on one side, the form on the other; stacked on mobile. What most of the BootForm
VitePress templates' own contact pages use:

```html
<div class="mx-auto grid max-w-4xl gap-12 px-6 py-16 md:grid-cols-2">

  <div class="flex flex-col gap-6">
    <h2 class="text-sm font-semibold uppercase tracking-widest opacity-50">Get in touch directly</h2>

    <div class="flex items-start gap-3">
      <span class="mt-0.5 text-brand-500">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"/><circle cx="12" cy="10" r="3"/></svg>
      </span>
      <div>
        <p class="font-medium">Based in Portland, OR</p>
        <a href="https://maps.google.com/?q=Portland,OR" target="_blank" rel="noopener" class="text-sm text-brand-500 hover:underline">Get directions ↗</a>
      </div>
    </div>

    <div class="flex items-start gap-3">
      <span class="mt-0.5 text-brand-500">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.362 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.338 1.85.573 2.81.7A2 2 0 0 1 22 16.92Z"/></svg>
      </span>
      <div>
        <p class="font-medium">Call or text</p>
        <a href="tel:+15555550123" class="text-sm text-brand-500 hover:underline">+1 (555) 555-0123</a>
      </div>
    </div>

    <div class="flex items-start gap-3">
      <span class="mt-0.5 text-brand-500">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>
      </span>
      <div>
        <p class="font-medium">Email</p>
        <a href="mailto:hello@example.com" class="text-sm text-brand-500 hover:underline">hello@example.com</a>
      </div>
    </div>
  </div>

  <div class="rounded-xl border border-black/10 bg-white p-6 shadow-sm dark:border-white/10 dark:bg-white/5 sm:p-8">
    <form action="https://f.bootform.com/{form_id}" method="POST" class="flex flex-col gap-4">
      <!-- fields -->
    </form>
  </div>

</div>
```

## Form + map

The form on one side, a location on the other, as an embedded Google Map instead of a plain
address line. `loading="lazy"` keeps an off-screen map from slowing down the rest of the page:

```html
<div class="mx-auto grid max-w-4xl gap-8 px-6 py-16 md:grid-cols-2">

  <div class="overflow-hidden rounded-xl border border-black/10 dark:border-white/10">
    <iframe
      src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d..."
      width="100%" height="100%" style="border:0" loading="lazy"
      referrerpolicy="no-referrer-when-downgrade"
      class="h-full min-h-[320px] w-full"
    ></iframe>
  </div>

  <div class="rounded-xl border border-black/10 bg-white p-6 shadow-sm dark:border-white/10 dark:bg-white/5 sm:p-8">
    <form action="https://f.bootform.com/{form_id}" method="POST" class="flex flex-col gap-4">
      <!-- fields -->
    </form>
  </div>

</div>
```

Get the real `src` value from Google Maps' own **Share → Embed a map** dialog for the actual
location; the `!1m18!1m12!1m3!1d...` string above is a placeholder, not a working map.

## Form + FAQ

A form next to (or below) a short FAQ, for a page where the form's job is catching whatever
questions the FAQ didn't answer, common on a pricing or support page:

```html
<div class="mx-auto grid max-w-5xl gap-12 px-6 py-16 md:grid-cols-2">

  <div class="flex flex-col gap-6">
    <h2 class="text-2xl font-bold tracking-tight">Common questions</h2>
    <details class="group rounded-lg border border-black/10 p-4 dark:border-white/10">
      <summary class="cursor-pointer list-none font-medium marker:content-none">
        How quickly do you reply?
      </summary>
      <p class="mt-2 text-sm opacity-70">Within a day, usually the same one.</p>
    </details>
    <details class="group rounded-lg border border-black/10 p-4 dark:border-white/10">
      <summary class="cursor-pointer list-none font-medium marker:content-none">
        Do you work with businesses outside the US?
      </summary>
      <p class="mt-2 text-sm opacity-70">Yes, remote work with any timezone.</p>
    </details>
  </div>

  <div class="rounded-xl border border-black/10 bg-white p-6 shadow-sm dark:border-white/10 dark:bg-white/5 sm:p-8">
    <h2 class="mb-4 text-2xl font-bold tracking-tight">Still have a question?</h2>
    <form action="https://f.bootform.com/{form_id}" method="POST" class="flex flex-col gap-4">
      <!-- fields -->
    </form>
  </div>

</div>
```

A plain `<details>`/`<summary>` needs no JavaScript for the open/close behavior at all; only add a
library if the design calls for an animated open/close transition, which `<details>` doesn't do
natively in most browsers yet.

## Newsletter band

A single-field, full-width band, usually placed between a page's main content and its footer, not
a full contact section:

```html
<div class="bg-brand-500 px-6 py-12 text-center">
  <h2 class="text-2xl font-bold tracking-tight text-white">Get updates by email</h2>
  <p class="mx-auto mt-2 max-w-md text-white/80">One email a month, no spam, unsubscribe anytime.</p>
  <form action="https://f.bootform.com/{form_id}" method="POST" class="mx-auto mt-6 flex max-w-md flex-col gap-2 sm:flex-row">
    <input name="email" type="email" required placeholder="you@example.com"
           class="flex-1 rounded-md border-0 bg-white px-3 py-2 outline-none focus:ring-2 focus:ring-white/50">
    <button type="submit" class="rounded-md bg-white px-5 py-2 font-medium text-brand-600 hover:bg-white/90">Subscribe</button>
  </form>
</div>
```

The input and button are inverted (white background, brand-colored text/no border) here, the same
deliberate exception used on every BootForm template's own call-to-action button: they sit on a
brand-colored surface, so inverting them is what keeps them legible, not a contradiction of the
usual pattern.
