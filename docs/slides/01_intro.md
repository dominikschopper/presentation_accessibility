## Who is actually affected?


- **Visual**: Blindness / Low vision / Colour blindness / Light sensitivity
- **Motor**: Tremor / Paralysis / Missing limbs
- **Cognitive**: Dyslexia / ADHD / Learning disabilities / Cognitive overload
- **Auditory**: Deafness / Hard of hearing
- **Temporary & situational**: Broken arm, shoulder ... / eye illnesses and problems

> ~15 % of the population have a permanent impairment.
> Temporary or situational ones affect everyone at some point.

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

## POUR Examples

- **Perceivable**:<br/>
  A fund's performance is not only indicated not by a green or red line, but also through text, captions,
  values, sufficient contrast, and screen reader information.
- **Operable**:<br/>
  A contact form can be used fully with a keyboard: input fields, checkboxes, error messages, and the submit
  button are accessible without a mouse.
- **Understandable**:<br/>
  Instead of __“Transaction failed,”__<!-- .element: class="c-orange" --> the message says:<br/>
  __“The transfer could not be completed because the IBAN is incomplete. Please check the IBAN.”__<!-- .element: class="c-green" --><br/>
  or<br/>
  A button is not labeled generically as __“Learn more”__<!-- .element: class="c-orange" -->
  but more specifically as<br/>
  __“View fund details”__<!-- .element: class="c-green" -->
- **Robust**:<br/>
  A button is technically a real `<button>` with the correct name, role, and state — not just a clickable `<div>`.
  This makes it work reliably with screen readers, voice control, future browsers or (in the best cases) even without JS.

---
## Accessibility is (like e.g. Security) a cross-cutting concern

- **we as developers** can help build a11y into our apps, but we need UX and BA
- **UX:** Behaviour of the components and the app in general as well as color contrast and Design
- **BA:** texts and structure of the app

Accessibility has to build into the App/Website/Forms from the ground up!

But we can make sure by using **semantic html**<sup>©️</sup>