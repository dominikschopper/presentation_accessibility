## Who is actually affected?

> ~15 % of the population have a permanent impairment.

- Visual: Blindness / Low vision / Colour blindness / Light sensitivity<!-- .element: class="fragment" -->
- Motor: Tremor / Paralysis / Missing limbs<!-- .element: class="fragment" -->
- Cognitive: Dyslexia / ADHD / Learning disabilities / Cognitive overload<!-- .element: class="fragment" -->
- Auditory: Deafness / Hard of hearing<!-- .element: class="fragment" -->
- Temporary & situational: Broken arm, shoulder ... / eye illnesses and problems<!-- .element: class="fragment" -->

---

<!-- .slide: id="business-case" -->
## The Business Case

### Germany
- **7.9 million** people with severe disabilities (Schwerbehinderung)
- ~10% of the German population
- Aging society: this number grows every year

### Global Context
- 1.3 billion people with disabilities worldwide (WHO)
- $8+ trillion annual disposable income

---

## Beyond Compliance
- Accessible sites rank better in search engines (SEO)
- Mobile-first design and a11y overlap significantly
- Usability improvements benefit everyone
- semantic HTML is easier to understand and read

> "Accessibility is not charity. It's good business."

---

<!-- .slide: id="legal" -->
## Barrierefreiheitsstärkungsgesetz (BFSG)

### Who Must Comply?
- All B2C digital products and services
- Online shops, banking apps, e-commerce

<small>(Exception: Microenterprises: <10 employees, <2M€ revenue)</small>

### Consequences
- Fines up to **€100,000** (from Competitors? Abmahnungen)
- Reputational damage

### Standard Required
- **WCAG 2.1 Level AA** (via EN 301 549)

> The question is not **IF** but **WHEN** this affects your project.

---

## WCAG 2.1 — The Standard

Four principles (**POUR**<!-- .element: class="c-blue" -->):

| Principle          | Meaning                         | Examples                              |
|--------------------|---------------------------------|---------------------------------------|
| **P**<!-- .element: class="c-blue" -->erceivable    | Content must be perceivable     | alt texts, contrast, captions         |
| **O**<!-- .element: class="c-blue" -->perable       | Usable without a mouse          | keyboard navigation, no time limits   |
| **U**<!-- .element: class="c-blue" -->nderstandable | Understandable                  | error messages, language, consistency |
| **R**<!-- .element: class="c-blue" -->obust         | Robust for assistive technology | valid HTML, ARIA used correctly       |

**Conformance levels:**
- **Level A** — Minimum; without it, content is inaccessible for some users
- <!-- .element: class="c-blue" -->
  **Level AA** — Target for most projects (and our project)
- **Level AAA** — Maximum accessibility; not always achievable

> European Accessibility Act require Level AA and german law follows that.

---

## A11y by Component Type

Three types of components — three areas of responsibility:

| Type                 | Role           | A11y Responsibility         |
|----------------------|----------------|-----------------------------|
| **Pages / Views**    | Route endpoint | Layout, Structure & Context |
| **Smart Components** | Business logic | Behavior & Feedback         |
| **Dumb Components**  | Pure UI        | Semantic Markup             |

---

## Pages / Views — Structure & Context

The page defines the **overall accessibility context**.

```vue
<template>
  <h1>{{ pageTitle }}</h1>           <!-- Exactly one h1 per page -->

  <main id="main-content">
    <!-- some content with valid headings structure -->
  </main>
</template>
```

### Page Responsibilities
- Set `document.title` on route change
- Define the `<h1>` and provide heading context
- Manage focus on navigation (focus `#main-content` or `<h1>`)
- Define landmark structure (`<main>`, `<nav>`, `<aside>`)

---

## Smart Components — Behavior & Feedback

Smart components handle **business logic** and **user feedback**.

```vue
<script setup>
const { announce } = useAnnouncer()
const { focusFirstError } = useFormA11y(formRef)

async function onSubmit() {
  const result = await validateAndSave()
  if (!result.valid) {
    focusFirstError()                 // Focus first invalid field
  } else {
    announce('Changes saved')         // Announce success accessibly
  }
}
</script>
```

### Smart Component Responsibilities
- Handle focus after async operations
- Manage errors
- Control modal/dialog open state and `inert` on background

---

## Dumb Components — Semantic Markup

Dumb components are **a11y-ready building blocks**.

```vue
<script setup>
defineProps<{
  label: string
  error?: string
  required?: boolean
}>()
</script>

<template>
  <div class="field">
    <label :for="id">
      {{ label }}
      <span v-if="required" aria-hidden="true"> *</span>
    </label>
    <input
      :id="id"
      :aria-invalid="error ? 'true' : undefined"
      :aria-describedby="error ? `${id}-error` : undefined"
      :aria-required="required"
    />
    <span v-if="error" :id="`${id}-error`" role="alert">
      {{ error }}
    </span>
  </div>
</template>
```

---

## Dumb Components — Rules

### Do
- Use semantic HTML (`<button>`, `<a>`, `<input>`)
- Set `aria-*` according to props
- handle focus via prop or `defineExpose`
- emit events

### Don't
- call backends or validate business logic
- Hard-code heading levels
- Manage global state (modals, toasts)

> Dumb components are portable. Smart components orchestrate them.

---

<!-- .slide: id="primevue-not-enough" -->
## PrimeVue Is Not Enough

Using PrimeVue? Great foundation, but...

### What PrimeVue Provides
- Semantic markup for components
- ARIA roles and states
- Keyboard interaction patterns

### What PrimeVue Does NOT Know
- Your page structure & heading hierarchy
- Your routing & focus management
- Your business logic & error handling
- Your content (alt texts, labels, announcements)

> An accessible component used incorrectly is still inaccessible.

---

## Accessibility is (like e.g. Security) a cross-cutting concern

- **we as developers** can help build a11y into our apps, but we need **UX** and **BA**
- **UX:** Behaviour of the components and the app in general as well as color contrast and Design
- **BA:** texts and structure of the app

**Accessibility has to be built into the App/Website/Forms from the ground up!**<!-- .element: class="c-orange fragment" -->

**But we can make sure by using _semantic html_<!-- .element: class="c-green" -->**<!-- .element: class="c-blue fragment" -->

---

## POUR → What We Cover

| Principle | Topics in this Presentation | WCAG Success Criteria |
|-----------|----------------------------|----------------------|
| **P**erceivable | [Contrast](#/contrast), Alt Texts, Labels | [1.1.1](https://www.w3.org/WAI/WCAG21/Understanding/non-text-content) / [1.4.3](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum) / [1.4.11](https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast) |
| **O**perable | [Keyboard Navigation](#/keyboard) / Focus Styles / Modals | [2.1.1](https://www.w3.org/WAI/WCAG21/Understanding/keyboard) / [2.4.7](https://www.w3.org/WAI/WCAG21/Understanding/focus-visible) / [2.4.11](https://www.w3.org/WAI/WCAG21/Understanding/focus-appearance) |
| **U**nderstandable | [Forms & Errors](#/forms) / Labels / Error Messages | [3.3.1](https://www.w3.org/WAI/WCAG21/Understanding/error-identification) / [3.3.2](https://www.w3.org/WAI/WCAG21/Understanding/labels-or-instructions) / [3.3.3](https://www.w3.org/WAI/WCAG21/Understanding/error-suggestion) |
| **R**obust | [Semantic HTML](#/semantic-html) / [Landmarks](#/landmarks) / [ARIA](#/aria) / [Live Regions](#/live-regions) | [4.1.1](https://www.w3.org/WAI/WCAG21/Understanding/parsing) / [4.1.2](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value) / [4.1.3](https://www.w3.org/WAI/WCAG21/Understanding/status-messages) |

> all links go to [Success Criteria for WCAG2.1](https://www.w3.org/WAI/WCAG21/Understanding/) sub chapters

---

## Where to Look It Up

- **WCAG 2.1 Quick Reference** (filterable by level & technique):<br/>
  https://www.w3.org/WAI/WCAG21/quickref/

- **Success Criteria for WCAG2.1**<br/>
  https://www.w3.org/WAI/WCAG21/Understanding/

- **ARIA Authoring Practices Guide** (patterns & examples):<br/>
  https://www.w3.org/WAI/ARIA/apg/

- **MDN Accessibility**:<br/>
  https://developer.mozilla.org/en-US/docs/Web/Accessibility

- **PrimeVue Accessibility Docs**:<br/>
  https://primevue.org/accessibility/
