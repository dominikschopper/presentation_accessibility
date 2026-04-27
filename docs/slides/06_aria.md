## The First Rule of ARIA

> "If you can use a native HTML element or attribute with the semantics and behaviour you require already built in, instead of re-purposing an element and adding an ARIA role, state or property — then do so."

- ARIA only changes the Accessibility Tree — **not** behaviour
- A `div[role=button]` still needs: `tabindex`, `keydown` handler, `Space` support …
- Every piece of native HTML semantics is more robust than retrofitted ARIA

**No ARIA is better than bad ARIA.**

---

## aria-label / aria-labelledby / aria-describedby

**BAD: Icon button without a label — screen reader reads nothing**<!-- .element: class="c-orange" -->
```html
<button @click="closeDialog">
  <svg>…</svg>
</button>
```

**GOOD: aria-label for icon-only elements**<!-- .element: class="c-green" -->
```html
<button @click="closeDialog" aria-label="Close dialog">
  <svg aria-hidden="true" focusable="false">…</svg>
</button>
```

**GOOD: aria-labelledby — reference to visible text**<!-- .element: class="c-green" -->
```html
<section aria-labelledby="settings-title">
  <h2 id="settings-title">Settings</h2>
</section>
```

**GOOD: aria-describedby — supplementary explanation**<!-- .element: class="c-green" -->
```html
<input aria-describedby="pw-hint" type="password" />
<p id="pw-hint">At least 8 characters, 1 number</p>
```

---

## aria-hidden — What Should Actually Be Hidden

**BAD: Focusable content behind aria-hidden**<!-- .element: class="c-orange" -->
```html
<div aria-hidden="true">
  <button>I am "invisible" in the AT — but still reachable by Tab</button>
</div>
```

**GOOD: Hide decorative elements only**<!-- .element: class="c-green" -->
```html
<svg aria-hidden="true" focusable="false">…</svg>

---

## Screen Reader Only / Visually Hidden

```
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
