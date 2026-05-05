## HTML is harder than it looks

"Everyone knows HTML" — true. Writing valid HTML is easy.

**Writing semantically correct HTML is hard:**
- Which element actually describes this content?
- When is `<article>` appropriate, when is `<div>`?
- Where does a navigation start and end?
- How do headings nest correctly across components?

HTML is the foundation of the Accessibility Tree.
If the HTML is wrong, ARIA attributes can do little to fix it.

> "The first rule of ARIA: don't use ARIA if native HTML does the job."

---

### Examples for Semantic HTML

If the data has a relation between rows and columns it should be in a table

```html
<table>
  <caption><!-- ... --></caption>
  <thead><!-- ... --></thead>
  <tbody><!-- ... --></tbody>
</table>
```
if it is a button use
```html
<button><!-- describe its function --></button>
```
if this button leads somewhere use
```html
<a href="/some-where"><!-- describes where it links to! --></a>
```

---

### Semantic Tags

- header, main, footer, aside, nav, article, section
- ul/ol + li, dl + dt,dd
- h1 - h6, p, cite, blockquote, abbr
- em, strong
- mark, time, code

### Some Examples

```html
<section aria-labelledby="main-aria-headline">
  <h3 id="main-aria-headline">Main Aria Quote</h3>
  <blockquote cite="https://www.w3.org/TR/using-aria/">
    "No ARIA is better than bad ARIA."
  </blockquote>
  <p>— <cite>Using ARIA</cite>, W3C</p>
</section>
```

---

### More Examples

```html
The <abbr title="S-Invest Manager">SIM</abbr> is a main platform project.
<time datetime="2025-01-23">23 January 2025</time>
Press <kbd>Ctrl</kbd> + <kbd>S</kbd>
The result is stored in <var>x</var>
```

```html
<details>
  <summary>What is WCAG?</summary>
  <p>The Web Content Accessibility Guidelines, published by the W3C...</p>
</details>
```
---

### Forms and Formgroups

```html
<fieldset>
  <legend>Check interesting topics?</legend>
    <label>
      <input type="checkbox" name="topics" value="a11y" /> Accessibility
    </label>
    <label>
      <input type="checkbox" name="topics" value="vue" /> Vue.js
    </label>
    <label>
      <input type="checkbox" name="topics" value="security" /> Web security
    </label>
</fieldset>
```

---

### More on Forms

```html
<fieldset>
  <legend>Personal information</legend>

  <div class="field-group">
    <label for="first-name">First name</label>
    <input id="first-name" type="text" autocomplete="given-name" />
  </div>

  <div class="field-group">
    <label for="last-name">Last name</label>
    <input id="last-name" type="text" autocomplete="family-name" />
  </div>

  <div class="field-group">
    <label for="email">Email address</label>
    <input id="email" type="email" autocomplete="email" />
    <p id="email-hint" class="hint">We will never share your email.</p>
  </div>

</fieldset>
```

possible **`autocomplete`** values are `given-name`<!-- .element: class="bg-palepink" -->,
`family-name`<!-- .element: class="bg-palepink" -->,
`email`<!-- .element: class="bg-palepink" -->,
`tel`<!-- .element: class="bg-palepink" -->,
`street-address`<!-- .element: class="bg-palepink" -->,
`postal-code`<!-- .element: class="bg-palepink" -->,
`current-password`<!-- .element: class="bg-palepink" -->

---

## The Accessibility Tree

The browser builds an **Accessibility Tree** in parallel with the DOM — this is what screen readers, voice control software, and Braille displays read.

Each DOM node has (or does not have):
- **Role** — what is it? (button, heading, landmark…)
- **Name** — what is it called? (label, alt text, aria-label…)
- **State** — what is its current state? (checked, expanded, disabled…)
- **Properties** — additional info (aria-level, aria-required…)

`<div>` and `<span>` have no semantic role → they barely exist in the Accessibility Tree.

`<section>` only is semantically relevant with an `aria-label`

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

