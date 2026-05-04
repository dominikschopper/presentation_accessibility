## Heading Hierarchy

**Bad — levels skipped, visual styling instead of semantics**
```html
<h1>App</h1>
<h3>Section</h3>   <!-- h2 missing! -->
<h5>Detail</h5>
```

**Good — strictly hierarchical**
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

E.g. now a component `<ArticleCard>` uses `<h2>` internally — but it should be `<h3>` or `<h4>` wherever it is used.

Don't style based on Tags only based on classes (this should be part of your `reset.css`!).

**Solution: pass the heading level down via `prop` or even better with `provide`/`inject`**

```ts
// headingLevel.ts — shared symbol
export const HEADING_LEVEL_KEY = Symbol('headingLevel')
```

```vue
<!-- DynHeading.vue — renders h1–h6 based on context -->
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

### Defining Heading Levels

we dont have to provide a prop to a component, it uses `<DynHeading>` and this
just "knows" what level it needs to be

```vue
<!-- HeadingIncrement.vue — increments the level for its slot content -->
<script setup lang="ts">
import { inject, provide } from 'vue'
import { HEADING_LEVEL_KEY } from './headingLevel'

const currentLevel = inject(HEADING_LEVEL_KEY, 1)
provide(HEADING_LEVEL_KEY, currentLevel + 1)
</script>
<template><slot /></template>
```

```vue
<!-- ArticleCard.vue — does not know its level, just uses DynHeading -->
<template>
  <article>
    <DynHeading>{{ title }}</DynHeading>     <!-- h? — comes from context -->
    <p>{{ excerpt }}</p>
  </article>
</template>
```

---

## Headings in Vue — Usage

You can now define `<h?>` hierarchy on the page level

```vue
<!-- SomePage.vue -->
<template>
  <h1>Some Page title</h1>                <!-- h1 -->

  <HeadingIncrement><!-- add 1 -->
    <DynHeading>Main area</DynHeading>    <!-- h2 -->

    <HeadingIncrement><!-- add 1 -->
      <ArticleCard />                     <!-- h3 inside -->

      <HeadingIncrement><!-- add 1 -->
        <DynHeading>Detail</DynHeading>   <!-- h4 -->
      </HeadingIncrement>
    </HeadingIncrement>
    <ArticleCard />                       <!-- h2 inside -->
  </HeadingIncrement>
</template>
```

Components are now hierarchically reusable without a hard-coded `h` tag.
