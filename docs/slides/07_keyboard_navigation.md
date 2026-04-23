## tabindex — Three Values

| Value | Meaning |
|------|-----------|
| `0` | Add to natural tab order |
| `-1` | Programmatically focusable, not reachable via Tab |
| `> 0` | **Never use** — destroys the natural tab order |

**✅ Interactive card: action via a real interactive element**
```html
<article class="card">
  <h3>Title</h3>
  <p>Description…</p>
  <a href="/detail/42">Read more</a>
</article>
```

---

## Focus Styles — Never Simply Remove Them

**❌ The classic anti-pattern**
```css
* { outline: none; }  /* millions of projects, one a11y crime */
```

**✅ Modern solution: distinguish mouse vs. keyboard**
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

The contrast requirement also applies to the focus ring: 3 : 1 against its surroundings (WCAG 2.4.11).

---

## Focus on Route Change in Vue

SPAs do not load a new page — the browser does not know the content has changed.
Screen readers stay silent, focus remains on the clicked link.

**✅ Move focus to main content after navigation**
```ts
// router/index.ts
router.afterEach((to) => {
  nextTick(() => {
    document.title = `${to.meta.title} – App name`

    const main = document.querySelector('#main-content') as HTMLElement
    main?.setAttribute('tabindex', '-1')
    main?.focus()
    main?.removeAttribute('tabindex')
  })
})
```

```html
<!-- App.vue -->
<main id="main-content">
  <RouterView />
</main>
```

