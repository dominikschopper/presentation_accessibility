## <kbd>Keyboard</kbd> all the things

the two rules of keyboard navigation:

- all interactive elements should be reachable by keyboard!<!-- .element: class="c-grey-light fragment highlight-red" -->
- all Elements that are reached by a keyboard should be highlighted!<!-- .element: class="c-grey-light fragment highlight-blue" -->

---

## tabindex — Three Values

Keyboard navigation is usually done with <kbd> &map; </kbd>

**e.g. interactive card: action via a real interactive element**

```html
<article class="card">
  <h3>Title</h3>
  <p>Description…</p>
  <a href="/detail/42">Read more</a>
</article>
```
manually set a `tabindex` to include/exclude elements from being reached
via keyboard

| Value | Meaning                                           |
|-------|---------------------------------------------------|
| `0`   | Add to natural tab order                          |
| `-1`  | Programmatically focusable, not reachable via Tab |
| `> 0` | **Never use** — destroys the natural tab order    |


---

## Focus Styles — Never Simply Remove Them

**The classic anti-pattern**
```css
* { outline: none; }  /* millions of projects, one a11y crime */
```

**Modern solution: distinguish mouse vs. keyboard**
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

The contrast requirement also applies to the focus ring: `3:1` against its surroundings (WCAG 2.4.11).

