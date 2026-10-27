# Icons

Icon weight, states, sizing, and direction: the details that make icons sit naturally in an interface.

Apply the shared [Design Token integration rules](SKILL.md#design-token-integration) to the recipes below.

## Match Icon Stroke to Text Weight

An icon next to text should carry the same optical weight as the text.

| Adjacent text | Icon stroke width (24px grid) |
| --- | --- |
| Regular (400), 14–16px | `1.5px` |
| Medium/Semibold (500–600) | `2px` |
| Bold (700), or emphasized standalone | `2.5px` |

Keep stroke weights consistent for equivalent text roles within the icon set; the weights above distinguish regular, semibold, and bold contexts. Size inline icons relative to the text's cap height, typically `1em`–`1.25em`.

## One SVG, Recolored per State

For monochrome icons whose state changes only in color or opacity, use one `currentColor` SVG and let CSS drive hover, selected, and disabled states. Strip hardcoded `fill` and `stroke` colors from those icons when importing them. Use the icon family's corresponding glyph when the state requires an outline-to-fill shape change.

```html
<svg fill="none" stroke="currentColor" stroke-width="2">…</svg>
```

```css
.icon-button { color: oklch(0.552 0.016 285.938); }
.icon-button:hover { color: oklch(0.21 0.006 285.885); }
.icon-button[aria-pressed="true"] { color: oklch(0.623 0.188 259.815); }
.icon-button:disabled { opacity: 0.4; }
```

## Outline Default, Fill Active

| Variant | Use for |
| --- | --- |
| Outline | Default state: toolbars, list rows, inline with text |
| Fill | Selected or active state: active tab, toggled bookmark, liked heart |

When animating a variant swap, apply [Motion Restraint](animations.md#motion-restraint) first, then use the [required icon animation values](animations.md#required-icon-animation-values).

## Design at Render Size

- Test every icon at the smallest size it will render, often `16px`.
- Prefer simplified glyphs for small contexts over scaled-down detailed artwork.
- Use the icon set's native grid sizes (`16`, `20`, `24`) rather than arbitrary fractional scales.
- Use SVG rather than raster assets.

## Icons in RTL

| Flip | Don't flip |
| --- | --- |
| Back/forward arrows, navigation chevrons | Logos and brand marks |
| Text alignment, lists, indent | Checkmarks |
| Directional send glyphs | Clocks, cups, pencils |
| Speaker waves tied to reading direction | Media playback controls |

```css
[dir="rtl"] .icon-directional {
  scale: -1 1;
}
```

Analyze composite icons part by part: an overlay may keep its position even when the base glyph flips.

## Accessible Icon Controls

Give every icon-only control an accessible name and mark purely decorative icons hidden from assistive technology.
