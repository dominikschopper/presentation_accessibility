## Heading Hierarchy

**❌ Bad — levels skipped, visual styling instead of semantics**
```html
<h1>App</h1>
<h3>Section</h3>   <!-- h2 missing! -->
<h5>Detail</h5>
```

**✅ Good — strictly hierarchical**
```html
<h1>App</h1>
  <h2>Section</h2>
    <h3>Detail</h3>
  <h2>Another section</h2>
```

Screen reader users navigate by headings — like a table of contents.
Firefox Accessibility Panel → "Check for issues" flags hierarchy errors directly.

---

## Headings in Vue — The Component Problem

`<ArticleCard>` uses `<h2>` internally — but depending on where it is embedded,
it should be `<h3>` or `<h4>`.

**Solution: pass the heading level down via `provide`/`inject`**

```ts
// headingLevel.ts — shared symbol
export const HEADING_LEVEL_KEY = Symbol('headingLevel')
```

```vue
<!-- HeadingSection.vue — increments the level for its slot content -->
<script setup lang="ts">
import { inject, provide } from 'vue'
import { HEADING_LEVEL_KEY } from './headingLevel'

const currentLevel = inject(HEADING_LEVEL_KEY, 1)
provide(HEADING_LEVEL_KEY, currentLevel + 1)
</script>
<template><slot /></template>
```

```vue
<!-- BaseHeading.vue — renders h1–h6 based on context -->
<script setup lang="ts">
import { inject, computed } from 'vue'
import { HEADING_LEVEL_KEY } from './headingLevel'

const level = inject(HEADING_LEVEL_KEY, 1)
const tag = computed(() => `h${Math.min(level, 6)}`)
</script>
<template>
  <component :is="tag"><slot /></component>
</template>
```

---

## Headings in Vue — Usage

```vue
<!-- Page.vue -->
<template>
  <h1>Page title</h1>                          <!-- h1 -->

  <HeadingSection>
    <BaseHeading>Main area</BaseHeading>        <!-- h2 -->

    <HeadingSection>
      <ArticleCard />                           <!-- h3 inside the card -->

      <HeadingSection>
        <BaseHeading>Detail</BaseHeading>       <!-- h4 -->
      </HeadingSection>
    </HeadingSection>
  </HeadingSection>
</template>
```

```vue
<!-- ArticleCard.vue — does not know its level, just uses BaseHeading -->
<template>
  <article>
    <BaseHeading>{{ title }}</BaseHeading>     <!-- h? — comes from context -->
    <p>{{ excerpt }}</p>
  </article>
</template>
```

Components are now hierarchically reusable without a hard-coded `h` tag.
