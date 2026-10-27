<a href="https://interfaces.dev/">
  <img width="320" height="168" alt="interfaces.dev" src="https://ho1jr3x2dcwdu3t5.public.blob.vercel-storage.com/interfaces-og-image.png" />
</a>

# Make Interfaces Feel Better

An agent skill for building and reviewing interface polish: typography, surfaces, motion, icons, and the small details that make interactions feel better.

## Install

```bash
npx skills add lazyseacow/make-interfaces-feel-better
```

## What it covers

| Area | Focus |
| --- | --- |
| [Typography](skills/make-interfaces-feel-better/typography.md) | Local information hierarchy, redundant text, necessary hints, text wrapping, font rendering, and tabular numbers |
| [Surfaces](skills/make-interfaces-feel-better/surfaces.md) | Concentric radii, optical alignment, shadows, structural borders, image outlines, and hit areas |
| [Animations](skills/make-interfaces-feel-better/animations.md) | Interruptible transitions, contextual entrances and exits, press feedback, and motion restraint |
| [Icons](skills/make-interfaces-feel-better/icons.md) | Stroke weight, state variants, `currentColor`, sizing, RTL behavior, and accessible icon controls |
| [Performance](skills/make-interfaces-feel-better/performance.md) | Explicit transition properties and targeted, measured use of `will-change` |

See [SKILL.md](skills/make-interfaces-feel-better/SKILL.md) for the core principles and complete review format.

## Use

Ask your agent to apply the skill, naming the components or user path to inspect:

```text
Use make-interfaces-feel-better in quick mode to review the checkout form.
```

```text
Use make-interfaces-feel-better in full mode to review src/components/settings.
```

| Mode | Coverage | Finding cap |
| --- | --- | --- |
| `quick` | Primary user path and highest-traffic states; HIGH and MEDIUM findings | 5 |
| `full` (default) | All five areas within the requested scope; HIGH, MEDIUM, and LOW findings | 15 |

Reviews include inspected evidence, actionable findings, considered but rejected changes, verification results, and a `Block`, `Needs changes`, or `Approve` verdict. Checks that were not run are identified explicitly.

## Project conventions

Changes follow the project's existing styling system and Design Token conventions for naming, layering, themes, and states. Matching tokens carry the fixed values prescribed by the skill.

Hierarchy and content refinements stay local to the requested components. They preserve permissions, business behavior, accessible names, and interaction flow while keeping necessary decision and error information near its controls. Motion follows the skill's interaction-frequency and reduced-motion conditions.

## Credits

Based on the original [make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better) skill by Jakub Krehel.

Upstream on skills.sh:

[![skills.sh](https://skills.sh/b/jakubkrehel/make-interfaces-feel-better)](https://skills.sh/jakubkrehel/make-interfaces-feel-better)
