## Contrast — WCAG Requirements

| Content                         | Level AA       | Level AAA |
|---------------------------------|----------------|-----------|
| Normal text (< 18pt)            | **4.5 : 1**    | 7 : 1     |
| Large text (≥ 18pt / 14pt bold) | **3 : 1**      | 4.5 : 1   |
| UI components & icons           | **3 : 1**      |     —     |
| Decorative elements, logos      | no requirement |     —     |

This affects not only colour-blind users — also: poor lighting, older monitors, age-related vision changes.

---

## Contrast — How to Check

**Browser tools**:

- Chrome DevTools → Elements → click a CSS colour → contrast ratio shown inline
- Firefox → Accessibility Panel → "Check for issues" → Contrast

**Extensions & tools**:

- **Colour Contrast Analyser** (desktop app): https://www.tpgi.com/color-contrast-checker/
- **axe DevTools** browser extension — automatically flags contrast failures
- **Who Can Use** — shows which user groups are excluded: https://www.whocanuse.com/

**In the design system**:

Contrast belongs in the design token documentation — not in the code review.

---

## Contrast — When It Fails

This is not a developer problem alone:

- Colours come from the design system / from UX
- Developers can check contrast, but cannot unilaterally change colours
- **Approach:** document the finding (screenshot + ratio), create an issue, involve the UX/design team
- Use the axe report as concrete input for the conversation

> "It looks nicer this way" is not a WCAG argument.
> With legal requirements (BFSG, EAA) it is not a good one too.
