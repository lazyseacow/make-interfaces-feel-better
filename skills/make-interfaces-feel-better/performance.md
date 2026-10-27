# Performance

Transition specificity and GPU compositing hints.

Apply the shared [Design Token integration rules](SKILL.md#design-token-integration) to the examples below.

## Transition Only What Changes

Never use `transition: all` or Tailwind's `transition-all`. Always specify the exact properties that change. Tailwind's bare `transition` maps to a curated default list of colors, opacity, shadow, and transforms, not to `all`; still prefer naming exactly what changes.

### Why

- `transition: all` forces the browser to watch every property for changes
- Causes unexpected transitions on properties you didn't intend to animate (colors, padding, shadows)
- Prevents browser optimizations

### CSS Example

```css
/* Good — only transition what changes */
.button {
  transition-property: scale, background-color;
  transition-duration: 150ms;
  transition-timing-function: ease-out;
}

/* Bad — transition everything */
.button {
  transition: all 150ms ease-out;
}
```

### Tailwind

```tsx
// Good — explicit properties
<button className="transition-[scale,background-color] duration-150 ease-out">

// Bad — transition all
<button className="transition-all duration-150 ease-out">
```

### Tailwind `transition-transform` Note

`transition-transform` in Tailwind maps to `transition-property: transform, translate, scale, rotate` — it covers all transform-related properties, not just `transform`. Use this when you're only animating transforms. For multiple non-transform properties, use the bracket syntax: `transition-[scale,opacity,filter]`.

## Use `will-change` Sparingly

`will-change` hints which properties are about to change so the browser can prepare rendering optimizations. Layer promotion is one possible optimization, rather than a guarantee.

For the animations covered by this skill, consider a targeted hint for `transform`, `opacity`, or `filter` only after observing a rendering problem. Effectiveness depends on the effect, browser, and device; keep the hint only when measurement shows an improvement. See [MDN's usage guidance](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/will-change).

### Rules

```css
/* Good — specific property that benefits from GPU compositing */
.animated-card {
  will-change: transform;
}

/* Good — multiple compositor-friendly properties */
.animated-card {
  will-change: transform, opacity;
}

/* Bad — never use will-change: all */
.animated-card {
  will-change: all;
}

/* Bad — properties that can't be GPU-composited anyway */
.animated-card {
  will-change: background-color, padding;
}
```

### Useful Properties

| Property | Compositing potential | When to consider `will-change` |
| --- | --- | --- |
| `transform` | Often composited | Only after a measured improvement |
| `opacity` | Often composited | Only after a measured improvement |
| `filter` (blur, brightness) | Depends on effect and browser | Only after profiling the actual effect |
| `clip-path` | Browser- and effect-dependent | Outside this skill's suggested hint set |
| `top`, `left`, `width`, `height` | Layout-affecting | Fix the animation approach first |
| `background`, `border`, `color` | Usually require painting | Skip speculative hints |

### When to Skip

Let the browser optimize normally when the animation is smooth. Add a hint only for an observed problem, and remove it when no longer needed. Avoid blanket hints: retained optimizations can increase memory use and rendering cost.
