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
