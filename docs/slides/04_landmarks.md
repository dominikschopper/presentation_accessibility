## Landmark Regions — Navigation for Screen Reader Users

Screen reader users do not read a page linearly —
they jump directly to landmarks using keyboard shortcuts.

```html
<header>                                      <!-- role="banner"        -->
  <nav aria-label="Main navigation"></nav>    <!-- role="navigation"    -->
</header>

<main id="main-content">                      <!-- role="main"          -->
  <section aria-labelledby="sec-title">
    <h2 id="sec-title">Section</h2>
  </section>
</main>

<aside aria-label="Related topics"></aside>   <!-- role="complementary" -->

<footer></footer>                             <!-- role="contentinfo"   -->
```

Multiple `<nav>` elements: always distinguish them with `aria-label` —
otherwise the user hears: _"Navigation. Navigation. Navigation."_

**`section`**<!-- .element class="c-orange" --> is the **`div`**<!-- .element class="c-blues" -->
 of landmarks. Only use it with an `aria-label` or `aria-labelledby` - otherwise it is like a `div`

---

## Landmarks — See Them Live

Pages with good landmark structure:
- https://www.w3.org/WAI/ — W3C reference implementation
- https://developer.mozilla.org/en-US/ — well-structured documentation site

How to inspect in the browser:
- Firefox Accessibility Panel: landmarks visible directly as a tree
- Chrome extension "Landmarks": https://matatk.agrip.org.uk/landmarks/ → overlay on the page

Screen reader shortcuts for landmarks:
- VoiceOver macOS: `Ctrl + Opt + U` → Rotor → Landmarks
- NVDA Windows: `D` = next landmark

