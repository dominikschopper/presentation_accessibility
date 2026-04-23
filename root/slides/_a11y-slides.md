# A11Y FTW
## Accessibility in Vue.js Frontends

Dominik Schöpper · Cofonpro

---

## Who is actually affected?

**Visual**
Blindness · Low vision · Colour blindness · Light sensitivity

**Motor**
Tremor · Paralysis · Missing limbs → no trackpad, no precise pointing, switch devices

**Cognitive**
Dyslexia · ADHD · Learning disabilities · Cognitive overload

**Auditory**
Deafness · Hard of hearing → less relevant on the web, but: videos without captions

**Temporary & situational**
Broken arm · Sunlight on screen · Child on your lap · Noise-cancelling headphones without audio

> ~15 % of the population have a permanent impairment.
> Temporary or situational ones affect everyone at some point.

---

## WCAG 2.1 — The Standard

Four principles (**POUR**):

| Principle | Meaning | Examples |
|---|---|---|
| **P**erceivable | Content must be perceivable | alt texts, contrast, captions |
| **O**perable | Usable without a mouse | keyboard navigation, no time limits |
| **U**nderstandable | Understandable | error messages, language, consistency |
| **R**obust | Robust for assistive technology | valid HTML, ARIA used correctly |

**Conformance levels:**
- **Level A** — Minimum; without it, content is inaccessible for some users
- **Level AA** — Target for most projects (and our project)
- **Level AAA** — Maximum accessibility; not always achievable

> BITV 2.0 (Germany) and the European Accessibility Act require Level AA.

---

# Contrast

---

## Contrast — WCAG Requirements

| Content | Level AA | Level AAA |
|---|---|---|
| Normal text (< 18pt) | **4.5 : 1** | 7 : 1 |
| Large text (≥ 18pt / 14pt bold) | **3 : 1** | 4.5 : 1 |
| UI components & icons | **3 : 1** | — |
| Decorative elements, logos | no requirement | — |

This affects not only colour-blind users — also: poor lighting, older monitors, age-related vision changes.

---

## Contrast — How to Check

**Browser tools**
- Chrome DevTools → Elements → click a CSS colour → contrast ratio shown inline
- Firefox → Accessibility Panel → "Check for issues" → Contrast

**Extensions & tools**
- **Colour Contrast Analyser** (desktop app): https://www.tpgi.com/color-contrast-checker/
- **axe DevTools** browser extension — automatically flags contrast failures
- **Who Can Use** — shows which user groups are excluded: https://www.whocanuse.com/

**In the design system**
Contrast belongs in the design token documentation — not in the code review.

---

## Contrast — When It Fails

This is not a developer problem alone:

- Colours come from the design system / from UX
- Developers can check contrast, but cannot unilaterally change colours
- **Approach:** document the finding (screenshot + ratio), create an issue, involve the UX/design team
- Use the axe report as concrete input for the conversation

> "It looks nicer this way" is not a WCAG argument.
> With legal requirements (BFSG, EAA) it is not a legal one either.

---

# Semantic HTML

---

## HTML is harder than it looks

"Everyone knows HTML" — true. Writing valid HTML is easy.

**Writing semantically correct HTML is hard:**
- Which element actually describes this content?
- When is `<section>` appropriate, when is `<div>`?
- Where does a navigation start and end?
- How do headings nest correctly across components?

HTML is the foundation of the Accessibility Tree.
If the HTML is wrong, ARIA attributes can do little to fix it.

> "The first rule of ARIA: don't use ARIA if native HTML does the job."

---

## The Accessibility Tree

The browser builds an **Accessibility Tree** in parallel with the DOM — this is what screen readers, voice control software, and Braille displays read.

Each DOM node has (or does not have):
- **Role** — what is it? (button, heading, landmark…)
- **Name** — what is it called? (label, alt text, aria-label…)
- **State** — what is its current state? (checked, expanded, disabled…)
- **Properties** — additional info (aria-level, aria-required…)

`<div>` and `<span>` have no semantic role → they barely exist in the Accessibility Tree.

---

## Inspecting the Accessibility Tree

**Chrome DevTools**
Elements panel → open the **Accessibility** tab on the right → click any node in the DOM

Or: DevTools → ⋮ → More Tools → **Accessibility**
→ "Enable full-page accessibility tree" → toggle the icon at the top of the Elements panel

**Firefox**
Tools → Browser Tools → **Accessibility**
→ full tree, issue highlighting, tab order visualisation

**To explore live:**
- https://www.accessibility-developer-guide.com/examples/sensible-aria-usage/
- https://dequeuniversity.com/library/

---

## Landmark Regions — Navigation for Screen Reader Users

Screen reader users do not read a page linearly —
they jump directly to landmarks using keyboard shortcuts.

```html
<header>                                      <!-- role="banner"        -->
  <nav aria-label="Main navigation"></nav>    <!-- role="navigation"    -->
</header>

<main id="main-content">                      <!-- role="main"          -->
  <section aria-labelledby="sec-title">
    <h2 id="sec-title">Section</h2>
  </section>
</main>

<aside aria-label="Related topics"></aside>   <!-- role="complementary" -->

<footer></footer>                             <!-- role="contentinfo"   -->
```

Multiple `<nav>` elements: always distinguish them with `aria-label` —
otherwise the user hears: _"Navigation. Navigation. Navigation."_

---

## Landmarks — See Them Live

Pages with good landmark structure:
- https://www.w3.org/WAI/ — W3C reference implementation
- https://developer.mozilla.org/en-US/ — well-structured documentation site

How to inspect in the browser:
- Firefox Accessibility Panel: landmarks visible directly as a tree
- Chrome extension "Landmarks": https://matatk.agrip.org.uk/landmarks/ → overlay on the page

Screen reader shortcuts for landmarks:
- VoiceOver macOS: `Ctrl + Opt + U` → Rotor → Landmarks
- NVDA Windows: `D` = next landmark

---

## Heading Hierarchy

**❌ Bad — levels skipped, visual styling instead of semantics**
```html
<h1>App</h1>
<h3>Section</h3>   <!-- h2 missing! -->
<h5>Detail</h5>
```

**✅ Good — strictly hierarchical**
```html
<h1>App</h1>
  <h2>Section</h2>
    <h3>Detail</h3>
  <h2>Another section</h2>
```

Screen reader users navigate by headings — like a table of contents.
Firefox Accessibility Panel → "Check for issues" flags hierarchy errors directly.

---

## Headings in Vue — The Component Problem

`<ArticleCard>` uses `<h2>` internally — but depending on where it is embedded,
it should be `<h3>` or `<h4>`.

**Solution: pass the heading level down via `provide`/`inject`**

```ts
// headingLevel.ts — shared symbol
export const HEADING_LEVEL_KEY = Symbol('headingLevel')
```

```vue
<!-- HeadingSection.vue — increments the level for its slot content -->
<script setup lang="ts">
import { inject, provide } from 'vue'
import { HEADING_LEVEL_KEY } from './headingLevel'

const currentLevel = inject(HEADING_LEVEL_KEY, 1)
provide(HEADING_LEVEL_KEY, currentLevel + 1)
</script>
<template><slot /></template>
```

```vue
<!-- BaseHeading.vue — renders h1–h6 based on context -->
<script setup lang="ts">
import { inject, computed } from 'vue'
import { HEADING_LEVEL_KEY } from './headingLevel'

const level = inject(HEADING_LEVEL_KEY, 1)
const tag = computed(() => `h${Math.min(level, 6)}`)
</script>
<template>
  <component :is="tag"><slot /></component>
</template>
```

---

## Headings in Vue — Usage

```vue
<!-- Page.vue -->
<template>
  <h1>Page title</h1>                          <!-- h1 -->

  <HeadingSection>
    <BaseHeading>Main area</BaseHeading>        <!-- h2 -->

    <HeadingSection>
      <ArticleCard />                           <!-- h3 inside the card -->

      <HeadingSection>
        <BaseHeading>Detail</BaseHeading>       <!-- h4 -->
      </HeadingSection>
    </HeadingSection>
  </HeadingSection>
</template>
```

```vue
<!-- ArticleCard.vue — does not know its level, just uses BaseHeading -->
<template>
  <article>
    <BaseHeading>{{ title }}</BaseHeading>     <!-- h? — comes from context -->
    <p>{{ excerpt }}</p>
  </article>
</template>
```

Components are now hierarchically reusable without a hard-coded `h` tag.

---

# ARIA

---

## The First Rule of ARIA

> "If you can use a native HTML element or attribute with the semantics and behaviour you require already built in, instead of re-purposing an element and adding an ARIA role, state or property — then do so."

- ARIA only changes the Accessibility Tree — **not** behaviour
- A `div[role=button]` still needs: `tabindex`, `keydown` handler, `Space` support …
- Every piece of native HTML semantics is more robust than retrofitted ARIA

**No ARIA is better than bad ARIA.**

---

## aria-label / aria-labelledby / aria-describedby

**❌ Icon button without a label — screen reader reads nothing**
```html
<button @click="closeDialog">
  <svg>…</svg>
</button>
```

**✅ aria-label for icon-only elements**
```html
<button @click="closeDialog" aria-label="Close dialog">
  <svg aria-hidden="true" focusable="false">…</svg>
</button>
```

**✅ aria-labelledby — reference to visible text**
```html
<section aria-labelledby="settings-title">
  <h2 id="settings-title">Settings</h2>
</section>
```

**✅ aria-describedby — supplementary explanation**
```html
<input aria-describedby="pw-hint" type="password" />
<p id="pw-hint">At least 8 characters, 1 number</p>
```

---

## aria-hidden — What Should Actually Be Hidden

**❌ Focusable content behind aria-hidden**
```html
<div aria-hidden="true">
  <button>I am "invisible" in the AT — but still reachable by Tab</button>
</div>
```

**✅ Hide decorative elements only**
```html
<svg aria-hidden="true" focusable="false">…</svg>

<!-- Star rating: visual vs. screen reader -->
<span aria-hidden="true">★★★☆☆</span>
<span class="sr-only">3 out of 5 stars</span>
```

```css
.sr-only {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}
```

---

# Keyboard Navigation & Focus

---

## tabindex — Three Values

| Value | Meaning |
|------|-----------|
| `0` | Add to natural tab order |
| `-1` | Programmatically focusable, not reachable via Tab |
| `> 0` | **Never use** — destroys the natural tab order |

**✅ Interactive card: action via a real interactive element**
```html
<article class="card">
  <h3>Title</h3>
  <p>Description…</p>
  <a href="/detail/42">Read more</a>
</article>
```

---

## Focus Styles — Never Simply Remove Them

**❌ The classic anti-pattern**
```css
* { outline: none; }  /* millions of projects, one a11y crime */
```

**✅ Modern solution: distinguish mouse vs. keyboard**
```css
/* Mouse users: no outline */
:focus:not(:focus-visible) {
  outline: none;
}
/* Keyboard users: clearly visible */
:focus-visible {
  outline: 3px solid #00b894;
  outline-offset: 3px;
  border-radius: 4px;
}
```

The contrast requirement also applies to the focus ring: 3 : 1 against its surroundings (WCAG 2.4.11).

---

## Focus on Route Change in Vue

SPAs do not load a new page — the browser does not know the content has changed.
Screen readers stay silent, focus remains on the clicked link.

**✅ Move focus to main content after navigation**
```ts
// router/index.ts
router.afterEach((to) => {
  nextTick(() => {
    document.title = `${to.meta.title} – App name`

    const main = document.querySelector('#main-content') as HTMLElement
    main?.setAttribute('tabindex', '-1')
    main?.focus()
    main?.removeAttribute('tabindex')
  })
})
```

```html
<!-- App.vue -->
<main id="main-content">
  <RouterView />
</main>
```

---

# Dynamic Content & Live Regions

---

## aria-live — Announcements for Screen Readers

**❌ Toast appears visually, but is silent for AT**
```html
<div class="toast" v-if="saved">Saved!</div>
```

**✅ With aria-live — the region must already be in the DOM on load**
```html
<!-- App.vue — always rendered, never v-if! -->
<p
  role="status"
  aria-live="polite"
  aria-atomic="true"
  class="sr-only"
>{{ announcement }}</p>
```

| Attribute | Value | When |
|---|---|---|
| `aria-live` | `polite` | Waits until AT is done — toasts, status messages |
| `aria-live` | `assertive` | Interrupts immediately — **only** for critical errors |

---

## aria-atomic — Reading the Right Portion

Without `aria-atomic`, the screen reader reads only the **changed part** of the content.

**Example: upload progress**
```html
<!-- atomic="false" (default): AT reads only "4" … "5" … "6" — no context -->
<p role="status" aria-live="polite" aria-atomic="false">
  {{ uploadedCount }} of {{ totalCount }} files uploaded
</p>
```

```html
<!-- atomic="true": AT reads "4 of 10 files uploaded" — understandable -->
<p role="status" aria-live="polite" aria-atomic="true">
  {{ uploadedCount }} of {{ totalCount }} files uploaded
</p>
```

**Rule of thumb:**
- `atomic="true"` — when the full text makes sense as a whole (status messages, counters)
- `atomic="false"` (default) — when only new entries matter (logs, chat)

---

## Live Region Composable (Vue 3)

```ts
// composables/useAnnouncer.ts
import { ref, nextTick } from 'vue'

const message = ref('')

export function useAnnouncer() {
  function announce(text: string) {
    // Force reset — otherwise AT won't re-read the same text
    message.value = ''
    nextTick(() => { message.value = text })
  }
  return { message, announce }
}
```

```html
<!-- App.vue — registered once globally -->
<p role="status" aria-live="polite" aria-atomic="true" class="sr-only">
  {{ message }}
</p>
```

```ts
// In any component
const { announce } = useAnnouncer()
async function save() {
  await api.save(data)
  announce('Changes have been saved.')
}
```

---

# Forms & Errors

---

## Label Association

**❌ Placeholder is not a label — disappears while typing**
```html
<input type="email" placeholder="Email address" />
```

**✅ Explicit association via `for`/`id`**
```html
<label for="email">Email address</label>
<input
  id="email"
  type="email"
  autocomplete="email"
  :aria-describedby="emailError ? 'email-error' : undefined"
  :aria-invalid="emailError ? 'true' : undefined"
/>
<span id="email-error" role="alert" v-if="emailError">
  {{ emailError }}
</span>
```

---

## Error Feedback — Visible AND Audible

**❌ Error appears visually, AT is not informed**
```html
<p class="error" v-if="error">{{ error }}</p>
```

**✅ role="alert" + focus on first invalid field after submit**

A reusable composable using `useTemplateRef` (Vue 3.5+):

```ts
// composables/useFormA11y.ts
import { useTemplateRef, nextTick } from 'vue'

export function useFormA11y(refName: string) {
  const formEl = useTemplateRef<HTMLFormElement>(refName)

  function focusFirstError() {
    nextTick(() => {
      const first = formEl.value?.querySelector<HTMLElement>('[aria-invalid="true"]')
      first?.focus()
    })
  }

  return { focusFirstError }
}
```

```html
<!-- ContactForm.vue -->
<form ref="contactForm">
  <input :aria-invalid="nameError ? 'true' : undefined" … />
  <input :aria-invalid="emailError ? 'true' : undefined" … />
  <div role="alert" aria-live="assertive" v-if="submitError">
    {{ submitError }}
  </div>
</form>
```

```ts
const { focusFirstError } = useFormA11y('contactForm')

async function submit() {
  const result = await validate()
  if (!result.valid) focusFirstError()
}
```

The composable works for any form — pass the template ref name, it handles the rest.

---

## Required Fields

```html
<label for="name">
  Full name
  <span aria-hidden="true"> *</span>
  <span class="sr-only">(required)</span>
</label>
<input
  id="name"
  type="text"
  required
  aria-required="true"
  aria-describedby="name-hint"
/>
<p id="name-hint" class="hint">
  First and last name as on your ID
</p>
```

---

# Modals & Dialogs

---

## Native `<dialog>` — What the Browser Handles

`showModal()` activates the native modal mode:

- **Focus trap** — Tab and Shift+Tab stay within the dialog
- **ESC** — closes the dialog automatically
- **Top layer** — sits above everything, no z-index juggling
- **Backdrop** — `::backdrop` pseudo-element, no extra overlay div needed

What the browser does **not** handle:
Making the rest of the page content inert for screen readers and Tab navigation.
That is what the `inert` attribute is for.

---

## Modal with `<dialog>`, Teleport and `inert`

```html
<!-- ConfirmDialog.vue -->
<Teleport to="body">
  <dialog
    ref="dialogEl"
    aria-labelledby="dialog-title"
    aria-describedby="dialog-desc"
    @close="onClose"
  >
    <h2 id="dialog-title">{{ title }}</h2>
    <p id="dialog-desc">{{ description }}</p>
    <div class="actions">
      <button @click="confirm">Confirm</button>
      <button @click="dialogEl?.close()">Cancel</button>
    </div>
  </dialog>
</Teleport>
```

```ts
const dialogEl = useTemplateRef<HTMLDialogElement>('dialogEl')
const triggerEl = ref<HTMLElement>()

watch(() => props.open, (val) => {
  const main = document.querySelector('#main-content') as HTMLElement
  if (val) {
    triggerEl.value = document.activeElement as HTMLElement
    main.inert = true           // blocks Tab AND AT access
    dialogEl.value?.showModal()
  } else {
    main.inert = false
    triggerEl.value?.focus()    // return focus to the triggering element
  }
})

function onClose() {
  emit('close')
}
```

---

## `inert` vs. `aria-hidden`

| | `aria-hidden="true"` | `inert` |
|---|---|---|
| AT access blocked | ✅ | ✅ |
| Tab focus blocked | ❌ | ✅ |
| Mouse click blocked | ❌ | ✅ |
| Browser support | excellent | good (Chrome 102+, FF 112+, Safari 15.5+) |

`inert` is the better choice for modal backgrounds — it blocks everything at once.
`aria-hidden` alone is not enough: focusable elements in the background remain reachable by Tab.

---

# Testing

---

## Tools

| Tool | What it checks |
|------|--------------|
| **axe DevTools** (extension) | ~30–40 % of WCAG criteria automatically |
| **Lighthouse** (DevTools) | A11y score, quick checks |
| **Firefox Accessibility Panel** | Tree, issues, tab order, contrast |
| **eslint-plugin-vuejs-accessibility** | A11y rules directly in the editor |
| **Screen reader** | What users actually hear |

```bash
pnpm add -D eslint-plugin-vuejs-accessibility
# eslint.config.ts:  'plugin:vuejs-accessibility/recommended'
```

---

## Screen Reader Quick Reference

| OS | Tool | Start |
|---|---|---|
| macOS | VoiceOver (built-in) | `Cmd + F5` |
| Windows | Narrator (built-in) | `Win + Ctrl + Enter` |
| Windows | NVDA (free) | nvaccess.org |
| Linux | Orca | varies by distro |

**Manual testing checklist:**
→ Keyboard only, no trackpad — Tab through all interactive elements
→ Open and close a dialog: does focus return correctly?
→ Submit a form with errors: what does the screen reader user hear?

---

## What Automated Tools Cannot Find

- Whether `alt` texts are *meaningful* — `alt="image123.jpg"` is technically valid
- Whether the tab order is *logical*
- Whether focus management after interactions *makes sense*
- Whether live region announcements *sound understandable*
- Whether the heading hierarchy is *semantically correct*

→ Automated tests find ~30–40 % of issues.
→ The rest requires manual testing — and ideally real users.

---

## Links

**Standards & Reference**
- WCAG 2.1 Quickref: https://www.w3.org/WAI/WCAG21/quickref/
- WAI-ARIA Patterns: https://www.w3.org/WAI/ARIA/apg/patterns/
- Accessibility Developer Guide: https://www.accessibility-developer-guide.com/

**Testing**
- axe DevTools: https://www.deque.com/axe/
- Who Can Use: https://www.whocanuse.com/
- Landmarks Extension: https://matatk.agrip.org.uk/landmarks/

**Vue-specific**
- eslint-plugin-vuejs-accessibility: https://github.com/vue-a11y/eslint-plugin-vuejs-accessibility

**Legal**
- BFSG (Germany): https://www.bmas.de/DE/Service/Gesetze-und-Gesetzesvorhaben/barrierefreiheitsstaerkungsgesetz.html
- European Accessibility Act: https://www.european-accessibility-act.de/

---

## Next Steps

1. Enable **eslint-plugin-vuejs-accessibility** — immediate feedback in the editor
2. Install **axe DevTools**, run through the app once, use the report as backlog input
3. Add **focus management** to the router hook
4. Set up a **live region** once globally in `App.vue`
5. Introduce **BaseHeading + HeadingSection** for new components
6. For new components: HTML semantics first, ARIA only where necessary

---

*A11Y FTW — Accessibility for the win*
