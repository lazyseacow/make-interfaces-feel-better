# Typography

Local information hierarchy and typography details that make interfaces feel better.

Apply the shared [Design Token integration rules](SKILL.md#design-token-integration) to the examples below.

## Local Hierarchy and Redundancy

Apply these checks to the components already in scope:

- Group related fields and actions using the project's spacing tokens and existing type roles. Make primary and secondary information distinguishable through spacing, alignment, and typography before adding explanatory copy.
- Remove optional secondary labels, descriptions, eyebrow text, subtitles, introductory paragraphs, decorative badges, and repeated status copy when they repeat nearby information or add nothing needed for a decision. Keep another occurrence when it serves a distinct decision point or accessible interaction.
- Keep page titles, field labels, and action names direct and concise. Express user-visible outcomes and constraints rather than internal implementation details.
- Animation examples describe how to group existing content; add only content that helps the user understand or act.

Reuse the state cues in Principles 18–19 of [SKILL.md](SKILL.md#18-one-svg-recolored-per-state); an existing selected style or meaningful icon can make another status sentence redundant.

## Necessary Information and Interaction Semantics

Retain concise information about costs, permissions, configuration limits, errors, and irreversible actions when users need it to decide or recover. Place it by the corresponding field or action using the project's existing hint, error, or description component and tokens.

For controls affected by a polish change, preserve accessible names, label associations, native roles, keyboard and focus behavior, and selected or expanded state semantics. If redundant helper text is removed, update its `aria-describedby` references while retaining necessary hint and error associations. Use the existing [icon accessibility guidance](icons.md#accessible-icon-controls) for icon-only controls.

## Text Wrapping

### text-wrap: balance

Distributes text evenly across lines, preventing orphaned words on headings and short text blocks. **Only works on blocks of 6 lines or fewer** (Chromium) or 10 lines or fewer (Firefox) — the balancing algorithm is computationally expensive, so browsers limit it to short text.

```css
/* Good — even line lengths on short text */
h1, h2, h3 {
  text-wrap: balance;
}
```

```css
/* Bad — default wrapping leaves orphans */
h1 {
  /* no text-wrap rule → "Read our
     blog" instead of balanced lines */
}
```

```css
/* Bad — balance on long paragraphs (silently ignored, wastes intent) */
.article-body p {
  text-wrap: balance;
}
```

**Tailwind:** `text-balance`

### text-wrap: pretty

Prevents orphaned words (a single word dangling on the last line) by adjusting line breaks throughout the paragraph. Unlike `balance`, it doesn't try to equalize line lengths — it just ensures the last line isn't embarrassingly short. Works on text of any length with no line-count limit.

This should be your **default for short-to-medium text** — paragraphs, descriptions, captions, list items, card text. For very long text (10+ lines), skip both `pretty` and `balance` — the browser's default wrapping is fine and you avoid unnecessary layout cost.

```css
/* Good — descriptions, captions, short paragraphs */
p, li, figcaption, blockquote {
  text-wrap: pretty;
}
```

```tsx
// Tailwind
<p className="text-pretty">
  A short paragraph that won't leave an orphan on the last line.
</p>
```

**Tailwind:** `text-pretty`

### When to Use Which

| Scenario | Use |
| --- | --- |
| Headings, titles where even distribution matters | `text-wrap: balance` |
| Short-to-medium text — paragraphs, descriptions, captions, UI text | `text-wrap: pretty` |
| Long text (10+ lines), code blocks, pre-formatted text | Neither — leave default |

## Font Smoothing (macOS)

On macOS, text renders heavier than intended by default. Apply antialiased smoothing to the root layout so all text renders crisper and thinner.

```css
/* CSS */
html {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

```tsx
// Tailwind — apply to root layout
<html className="antialiased">
```

### Good vs. Bad

```css
/* Good — applied once at the root */
html {
  -webkit-font-smoothing: antialiased;
}

/* Bad — applied per-element, inconsistent */
.heading {
  -webkit-font-smoothing: antialiased;
}
.body {
  /* no smoothing → heavier than heading */
}
```

**Note:** This only affects macOS rendering. Other platforms ignore these properties, so it's safe to apply universally.

## Font Family Scope

This skill does not require a specific font family. Do not introduce a paid or proprietary typeface just to satisfy the polish checklist.

Use the product's existing type system unless the task explicitly asks for a type change. If the design calls for a system-native macOS feel, use the system font stack. If the design calls for a commercial face such as Helvetica Now, treat it as an optional brand decision and keep a practical fallback stack.

```css
/* System-native macOS/iOS feel */
html {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}
```

```css
/* Commercial brand face with safe fallbacks */
html {
  font-family: "Helvetica Now", "Helvetica Neue", Arial, sans-serif;
}
```

**Rule:** font smoothing, text wrapping, and tabular numbers are rendering details. They do not override the project's chosen font family.

## Tabular Numbers

When numbers update dynamically (counters, prices, timers, table columns), use tabular-nums to make all digits equal width. This prevents layout shift as values change.

```css
/* CSS */
.counter {
  font-variant-numeric: tabular-nums;
}
```

```tsx
// Tailwind
<span className="tabular-nums">{count}</span>
```

### When to Use

| Use tabular-nums | Don't use tabular-nums |
| --- | --- |
| Counters and timers | Isolated static display numbers |
| Prices that update | Decorative large numbers |
| Table columns with numbers | Phone numbers, zip codes |
| Animated number transitions | Version numbers (v2.1.0) |
| Scoreboards, dashboards | |

### Caveat

Some fonts (like Inter) change the visual appearance of numerals with this property — specifically, the digit `1` becomes wider and centered. This is expected behavior and usually desirable for alignment, but verify it looks right in your specific font.

```css
/* With Inter font:
   Default:  1234  → proportional, "1" is narrow
   Tabular:  1234  → all digits equal width, "1" centered */
```
