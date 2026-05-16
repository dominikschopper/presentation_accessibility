## Appendix

Additional reference material

---

### Semantic HTML — Tag Reference

- `header`, `main`, `footer`, `aside`, `nav`, `article`, `section`
- `ul`/`ol` + `li`, `dl` + `dt`, `dd`
- `h1`–`h6`, `p`, `cite`, `blockquote`, `abbr`
- `em`, `strong`
- `mark`, `time`, `code`

### Example: Labeled section with blockquote

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

### Inline Semantic Elements

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
