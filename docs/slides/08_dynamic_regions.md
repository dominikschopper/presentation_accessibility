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

