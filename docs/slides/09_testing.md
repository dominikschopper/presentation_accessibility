## Tools

| Tool | What it checks |
|------|--------------|
| **axe DevTools** (extension) | ~30–40 % of WCAG criteria automatically |
| **Lighthouse** (DevTools) | A11y score, quick checks |
| **Firefox Accessibility Panel** | Tree, issues, tab order, contrast |
| **eslint-plugin-vuejs-accessibility** | A11y rules directly in the editor |
| **Screen reader** | What users actually hear |

```bash
pnpm add -D eslint-plugin-vuejs-accessibility
# eslint.config.ts:  'plugin:vuejs-accessibility/recommended'
```

---

## Screen Reader Quick Reference

| OS | Tool | Start |
|---|---|---|
| macOS | VoiceOver (built-in) | `Cmd + F5` |
| Windows | Narrator (built-in) | `Win + Ctrl + Enter` |
| Windows | NVDA (free) | nvaccess.org |
| Linux | Orca | varies by distro |

**Manual testing checklist:**
→ Keyboard only, no trackpad — Tab through all interactive elements
→ Open and close a dialog: does focus return correctly?
→ Submit a form with errors: what does the screen reader user hear?

---

## What Automated Tools Cannot Find

- Whether `alt` texts are *meaningful* — `alt="image123.jpg"` is technically valid
- Whether the tab order is *logical*
- Whether focus management after interactions *makes sense*
- Whether live region announcements *sound understandable*
- Whether the heading hierarchy is *semantically correct*

→ Automated tests find ~30–40 % of issues.
→ The rest requires manual testing — and ideally real users.

