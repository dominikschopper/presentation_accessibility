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

## Examples for Semantic HTML

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
<button><!-- ... --></button>
```
if this button leads somewhere use
```html
<a href="/some-where"><!-- ... --></a>
```

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

