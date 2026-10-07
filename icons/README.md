# Carousell Icon Library

Icons from Carousell's Figma design system, served as static assets from
`public/icons/`. Browse them all at **[/foundation/iconography](/foundation/iconography)** —
search, switch size, click to copy a path.

Sourced from Figma file [`bEX9lgvC7uMtLTXrfwQbUO`](https://www.figma.com/design/bEX9lgvC7uMtLTXrfwQbUO/Icons).

| Library | Path | Icons | Sizes | What it's for |
|---|---|---|---|---|
| **System** | `system/` | 206 | 16 / 24 / 32px | Interface glyphs — actions, navigation, states |
| **Object** | `object/` | 96 | mixed (24 / 32 / 56px) | Illustrative artwork — dialogs, empty states, badges |

## Using an icon

Both libraries live under `public/`, so they are **static files served by URL** —
not modules. There is no svgr in this project; `import Icon from "./foo.svg"`
will not work.

### System icons

Use the `SystemIcon` component, which picks the size-matched export for you:

```tsx
import { SystemIcon } from "@/components/design-system/icons/system-icon"

<SystemIcon name="action/edit" size="sm" />                        // 24px export
<SystemIcon name="navigation/chevron_right" size="xs" />            // 16px export
<SystemIcon name="state/alert_filled" size={32} alt="Warning" />    // 32px export
```

`size` takes `xs` (16), `sm`/`md` (24), `lg` (32), or a pixel number that snaps
to the nearest export. Omit `alt` for decorative icons — they are then hidden
from assistive tech.

### Object icons

Sizes vary per icon, so reference these by path:

```tsx
<img src="/icons/object/dialog/icon_dialog_success.svg" alt="" className="h-[100px] w-[167px]" />
```

### Colour

These are static SVGs with colours baked in — `currentColor` and Tailwind text
colours have no effect. System icons carry a `#57585A` stroke. For a white icon
on a dark or brand fill, use the filter pair:

```tsx
<SystemIcon name="action/edit" size="sm" className="brightness-0 invert" />
```

For a selected or active state, prefer the `_filled` variant where one exists
(`social/like` → `social/like_filled`).

## Adding icons

1. In Figma, select the icons and export as **SVG**.
2. Drop the files into `public/icons/system/[category]/`, named
   `[size]_[snake_case_name].svg` — all lowercase, underscores only, no spaces.
3. That's it. `lib/get-icons.ts` reads the folders at build time, so
   `/foundation/iconography` picks them up with no code change. A brand-new
   category folder appears on its own.

```
public/icons/system/action/16_add_photo.svg
public/icons/system/action/24_add_photo.svg
public/icons/system/action/32_add_photo.svg
```

### Check your export

Figma exports need two things watched:

- **Frame backgrounds.** If the Figma frame has a fill, it exports as a
  full-bleed `<rect>` before the artwork and the icon renders on a grey box.
  All 97 affected object icons were cleaned up; check new exports for
  `<rect width="…" height="…" fill="#EBEBEB"/>` right after the `<svg>` tag.
- **viewBox rounding.** Confirm the `viewBox` is square and matches the size
  prefix. 13 system icons came through off by one (`action/32_camera.svg` is
  `33x32`) from bounding-box rounding — harmless but slightly off-centre.

## Legacy files

The ~200 loose SVGs at the root of `public/icons/` predate both libraries; they
arrived piecemeal with individual prototypes. Five are still referenced
(`56-Photography-c.svg`, `Shop.svg`, `Help.svg`, `caroubiz.svg`). Prefer the
`system/` equivalent for anything new — most of the root files are unsized
duplicates of icons now in `system/`.
