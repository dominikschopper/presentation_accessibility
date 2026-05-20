<!-- .slide: id="forms" -->
# Forms & Errors — <span class="c-blue">[U]</span> Understandable

WCAG [3.3.1](https://www.w3.org/WAI/WCAG21/Understanding/error-identification), [3.3.2](https://www.w3.org/WAI/WCAG21/Understanding/labels-or-instructions) & [3.3.3](https://www.w3.org/WAI/WCAG21/Understanding/error-suggestion)

---

## Label Association

**Placeholder is not a label — disappears while typing**
```html
<input type="email" placeholder="Email address" />
```

**Explicit association via `for`/`id`**
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

## Error Feedback — Visible AND Audible 1/3

**Error appears visually, AT is not informed**
```html
<p class="error" v-if="error">{{ error }}</p>
```

---

## Error Feedback — Visible AND Audible 2/3

**role="alert" + focus on first invalid field after submit**

A reusable composable using `useTemplateRef` (Vue 3.5+):

```ts
// composables/useFormA11y.ts
import { type Ref, nextTick } from 'vue'

export function useFormA11y(formEl: Ref<HTMLFormElement | null>) {
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

---

## Error Feedback — Visible AND Audible 3/3

```ts
const { focusFirstError } = useFormA11y(useTemplateRef('contactForm'));

async function submit() {
  const result = await validate();
  if (!result.valid) {
    focusFirstError();
  }
}
```

The composable works for any form — pass the template ref, it handles the rest.

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

<!-- .slide: id="modals" -->
# Modals & Dialogs — <span class="c-blue">[O]</span> Operable

WCAG [2.1.1](https://www.w3.org/WAI/WCAG21/Understanding/keyboard) & [2.4.3](https://www.w3.org/WAI/WCAG21/Understanding/focus-order)

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
---

failsafe `inert` on main

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

|                     | `aria-hidden="true"` | `inert` |
|---------------------|----------------------|---------|
| AT access blocked   |           Yes        |   Yes   |
| Tab focus blocked   |           No         |   Yes   |
| Mouse click blocked |           No         |   Yes   |
| Browser support     |       excellent.     |  good<br/>(Chrome 102+, FF 112+, Safari 15.5+) |

`inert` is the better choice for modal backgrounds — it blocks everything at once.
`aria-hidden` alone is not enough: focusable elements in the background remain reachable by Tab.
