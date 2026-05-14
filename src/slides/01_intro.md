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
- Fines up to **€100,000** or Competitors (Abmahnungen)
- Reputational damage

### Standard Required
- **WCAG 2.1 Level AA** (via EN 301 549)

> The question is not **IF** but **WHEN** this affects your project.

---

<!-- .slide: id="primevue-not-enough" -->
## PrimeVue Is Not Enough

Using PrimeVue? Great foundation, but...

### What PrimeVue Provides
- Semantic markup for components ✓
- ARIA roles and states ✓
- Keyboard interaction patterns ✓

---

### What YOU Still Own

| Your Responsibility | Example                                     |
|---------------------|---------------------------------------------|
| Heading hierarchy   | Is your `<Card>` title an `<h2>` or `<h4>`? |
| Color contrast      | Your theme colors, not PrimeVue's &rarr; UX |
| Alt texts           | Your images, your content                   |
| Tab order           | Your page layout                            |
| Focus management    | Your modal/drawer flows &rarr; UX           |
| Live announcements  | Your form validation, your toasts           |

> An accessible component used incorrectly is still inaccessible.

---

## Who is actually affected?

> ~15 % of the population have a permanent impairment.

- Visual: Blindness / Low vision / Colour blindness / Light sensitivity<!-- .element: class="fragment" -->
- Motor: Tremor / Paralysis / Missing limbs<!-- .element: class="fragment" -->
- Cognitive: Dyslexia / ADHD / Learning disabilities / Cognitive overload<!-- .element: class="fragment" -->
- Auditory: Deafness / Hard of hearing<!-- .element: class="fragment" -->
- Temporary & situational: Broken arm, shoulder ... / eye illnesses and problems<!-- .element: class="fragment" -->

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

## POUR Examples 1/2

- **Perceivable**:<br/>
  A fund's performance is not only indicated not by a green or red line, but also through text, captions,
  values, sufficient contrast, and screen reader information.
- **Operable**:<br/>
  A contact form can be used fully with a keyboard: input fields, checkboxes, error messages, and the submit
  button are accessible without a mouse.

---

## POUR Examples 2/2

- **Understandable**:<br/>
  Instead of __“Transaction failed,”__<!-- .element: class="c-orange" --> the message says:<br/>
  __“The transfer could not be completed because the IBAN is incomplete. Please check the IBAN.”__<!-- .element: class="c-green" --><br/>
  or<br/>
  A button is not labeled generically as __“Learn more”__<!-- .element: class="c-orange" -->
  but more specifically as<br/>
  __“View fund details”__<!-- .element: class="c-green" -->
- **Robust**:<br/>
  E.g. a button works in any browser/screen reader is technically a real `<button>` with the correct name, role, and state —
  not just a clickable `<div>`.

---
## Accessibility is (like e.g. Security) a cross-cutting concern

- **we as developers** can help build a11y into our apps, but we need **UX** and **BA**
- **UX:** Behaviour of the components and the app in general as well as color contrast and Design
- **BA:** texts and structure of the app

Accessibility has to build into the App/Website/Forms from the ground up!

But we can make sure by using **semantic html**<sup>©️</sup>