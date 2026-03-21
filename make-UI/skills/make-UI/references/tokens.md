# SnowUI Design Tokens

Complete 131-variable token system. All collections, all modes, all values.
Source: `docs/patterns/raw/design-tokens.json` + `docs/patterns/design-system-foundations.md`

---

## Colors (4 modes: SnowUI-Light, SnowUI-Dark, iOS-Light, iOS-Dark)

### Backgrounds

| Token | SnowUI-Light | SnowUI-Dark | iOS-Light | iOS-Dark |
|-------|-------------|-------------|-----------|----------|
| Background/1 | `#ffffff` | `#333333` | `#f5f5f6` | `#333333` |
| Background/2 | `#f9f9fa` | `#ffffff0a` | `#ffffff` | `#ffffff0a` |
| Background/3 | `#ffffffcc` | `#0000001a` | `#00000008` | `#0000001a` |
| Background/4 | `#edeefc` | `#edeefc` | `#00000005` | `#0000001a` |
| Background/5 | `#e6f1fd` | `#e6f1fd` | `#ffffff` | `#ffffffcc` |
| Background/6 | `#ffffffe5` | `#404040e5` | `#ffffffe5` | `#404040e5` |

### Black Opacity Scale (inverts to White in dark mode)

| Token | SnowUI-Light | SnowUI-Dark | iOS-Light | iOS-Dark |
|-------|-------------|-------------|-----------|----------|
| Black/100% | `#000000` | `#ffffff` | `#000000` | `#ffffff` |
| Black/80% | `#000000cc` | `#ffffffcc` | `#000000cc` | `#ffffffcc` |
| Black/40% | `#00000066` | `#ffffff66` | `#00000066` | `#ffffff66` |
| Black/20% | `#00000033` | `#ffffff33` | `#00000033` | `#ffffff33` |
| Black/10% | `#0000001a` | `#ffffff26` | `#0000001a` | `#ffffff26` |
| Black/4% | `#0000000a` | `#ffffff1a` | `#0000000a` | `#ffffff1a` |

### White Opacity Scale (inverts to Black in dark mode)

| Token | SnowUI-Light | SnowUI-Dark | iOS-Light | iOS-Dark |
|-------|-------------|-------------|-----------|----------|
| White/100% | `#ffffff` | `#000000` | `#ffffff` | `#000000` |
| White/80% | `#ffffffcc` | `#000000cc` | `#ffffffcc` | `#000000cc` |
| White/40% | `#ffffff66` | `#00000066` | `#ffffff66` | `#00000066` |
| White/20% | `#ffffff33` | `#00000033` | `#ffffff33` | `#00000033` |
| White/10% | `#ffffff1a` | `#0000001a` | `#ffffff1a` | `#0000001a` |
| White/4% | `#ffffff0a` | `#0000000a` | `#ffffff0a` | `#0000000a` |

### Primary + Secondary Palette

| Token | SnowUI-Light | SnowUI-Dark | iOS-Light | iOS-Dark |
|-------|-------------|-------------|-----------|----------|
| Primary | alias: Black/100% | alias: Secondary/Indigo | alias: Secondary/Blue | alias: Secondary/Blue |
| Secondary/Indigo | `#adadfb` | `#adadfb` | `#5856d6` | `#5e5ce6` |
| Secondary/Purple | `#b899eb` | `#b899eb` | `#af52de` | `#bf5af2` |
| Secondary/Cyan | `#a0bce8` | `#a0bce8` | `#32ade6` | `#64d2ff` |
| Secondary/Blue | `#7dbbff` | `#7dbbff` | `#007aff` | `#0a84ff` |
| Secondary/Green | `#71dd8c` | `#71dd8c` | `#34c759` | `#30d158` |
| Secondary/Mint | `#6be6d3` | `#6be6d3` | `#00c7be` | `#63e6e2` |
| Secondary/Yellow | `#ffcc00` | `#ffcc00` | `#ffcc00` | `#ffd60a` |
| Secondary/Orange | `#ffb55b` | `#ffb55b` | `#ff9500` | `#ff9f0a` |
| Secondary/Red | `#ff4747` | `#ff4747` | `#ff3b30` | `#ff453a` |
| Logo 1 | `#4c98fd` | `#4c98fd` | alias: Primary | alias: Primary |
| Logo 2 | `#4f507f` | `#ededff` | `#4f507f` | `#ededff` |

### iOS Semantic Label Tokens (used in mobile patterns)

| Token | Light | Dark | Maps To |
|-------|-------|------|---------|
| Labels/Primary | `rgba(0,0,0,1)` | `rgba(255,255,255,1)` | ≈ Black/100% |
| Labels/Secondary | `rgba(0,0,0,0.4)` | `rgba(255,255,255,0.4)` | ≈ Black/40% |
| Labels/Tertiary | `rgba(0,0,0,0.2)` | `rgba(255,255,255,0.2)` | ≈ Black/20% |
| Colors/Blue | `#007AFF` | `#0A84FF` | iOS system blue |
| Fills/Tertiary | `rgba(120,120,128,0.12)` | — | iOS search field bg |
| Overlays/Default | `rgba(0,0,0,0.2)` | — | Alert dialog overlay |

### Static Color Styles (do NOT change with theme)

| Style | Hex | Opacity |
|-------|-----|---------|
| StaticBlack/100 | `#000000` | 1.0 |
| StaticBlack/80 | `#000000` | 0.8 |
| StaticBlack/40 | `#000000` | 0.4 |
| StaticBlack/20 | `#000000` | 0.2 |
| StaticBlack/10 | `#000000` | 0.1 |
| StaticBlack/4 | `#000000` | 0.04 |
| StaticWhite/100 | `#ffffff` | 1.0 |
| StaticWhite/80 | `#ffffff` | 0.8 |
| StaticWhite/40 | `#ffffff` | 0.4 |
| StaticWhite/20 | `#ffffff` | 0.2 |
| StaticWhite/10 | `#ffffff` | 0.1 |
| StaticWhite/4 | `#ffffff` | 0.04 |

### Gradient Styles

All secondary gradients share the same overlay: `linear-gradient(180deg, rgba(255,255,255,0.05) 0%, rgba(255,255,255,0.4) 100%)` applied over a base color.

| Style | Base Color |
|-------|-----------|
| Gradient/Primary | `#000000` |
| Gradient/Gray | `#f9f9fa` (overlay: `rgba(0,0,0,0.03)` to `rgba(0,0,0,0.01)`) |
| Gradient/Black | `rgba(0,0,0,0.2)` |
| Gradient/Purple | `#b899eb` |
| Gradient/Indigo | `#adadfb` |
| Gradient/Blue | `#7dbbff` |
| Gradient/Cyan | `#a0bce8` |
| Gradient/Mint | `#6be6d3` |
| Gradient/Green | `#71dd8c` |
| Gradient/Yellow | `#ffcc00` |
| Gradient/Orange | `#ffb55b` |
| Gradient/Red | `#ff4747` |

---

## Spacing (4 modes: Standard, Expanded+4, Expanded+8, Condensed-4)

| Token | Standard | Expanded(+4) | Expanded(+8) | Condensed(-4) |
|-------|----------|-------------|-------------|--------------|
| 0 | 0 | 0 | 0 | 0 |
| 4 | 4 | 8 | 12 | 0 |
| 8 | 8 | 12 | 16 | 4 |
| 12 | 12 | 16 | 20 | 8 |
| 16 | 16 | 20 | 24 | 12 |
| 20 | 20 | 24 | 28 | 16 |
| 24 | 24 | 28 | 32 | 20 |
| 28 | 28 | 32 | 36 | 24 |
| 32 | 32 | 36 | 40 | 28 |
| 40 | 40 | 44 | 48 | 36 |
| 48 | 48 | 52 | 56 | 44 |

Applies to: Gap, Padding, Margin. All values are multiples of 4.

---

## Corner Radius (4 modes: Standard, Expanded+4, Expanded+8, Condensed-4)

| Token | Standard | Expanded(+4) | Expanded(+8) | Condensed(-4) |
|-------|----------|-------------|-------------|--------------|
| 0 | 0 | 0 | 0 | 0 |
| 4 | 4 | 8 | 12 | 0 |
| 8 | 8 | 12 | 16 | 4 |
| 12 | 12 | 16 | 20 | 8 |
| 16 | 16 | 20 | 24 | 12 |
| 20 | 20 | 24 | 28 | 16 |
| 24 | 24 | 28 | 32 | 20 |
| 28 | 28 | 32 | 36 | 24 |
| 32 | 32 | 36 | 40 | 28 |
| 40 | 40 | 44 | 48 | 36 |
| 48 | 48 | 52 | 56 | 44 |
| 80 | 80 | 84 | 88 | 76 |
| Full | 9999 | 9999 | 9999 | 9999 |

Applies to: vector elements, components, blocks, frames, pages.

---

## CHART: Line Corner Radius (3 modes: Small, Medium, Big)

Local to chart components — NOT in the shared SnowUI library.

| Token | Small | Medium | Big |
|-------|-------|--------|-----|
| Corner radius | 2 | 16 | 28 |

Applies to: bar chart rectangle corners, line chart stroke caps. Small = angular/sharp joins, Medium = smooth default curves, Big = very rounded lines.

---

## CHART: Dot Size (3 variables × 3 modes)

Local to chart components — NOT in the shared SnowUI library.

| Token | Small | Medium | Big |
|-------|-------|--------|-----|
| Dot | 4 | 8 | 10 |
| Dot 2 | 8 | 16 | 20 |
| Dot 3 | 10 | 20 | 24 |

Applies to: scatter chart dots, line chart data points, legend indicator dots. Use Dot for inline legends, Dot 2 for chart data points, Dot 3 for interactive/hover-enlarged points.

---

## Size (3 modes: Default, Small-4, Large+4)

| Token | Default | Small(-4) | Large(+4) |
|-------|---------|-----------|-----------|
| 16 | 16 | 12 | 20 |
| 20 | 20 | 16 | 24 |
| 24 | 24 | 20 | 28 |
| 28 | 28 | 24 | 32 |
| 32 | 32 | 28 | 36 |
| 40 | 40 | 36 | 44 |
| 48 | 48 | 44 | 52 |
| 64 | 54 | 50 | 58 |
| 80 | 80 | 76 | 84 |
| 100 | 100 | 96 | 104 |
| 124 | 124 | 120 | 128 |

Applies to: icons, avatars, logos, images, emoji.

---

## Font (7 modes)

| Mode | Font Family |
|------|-----------|
| Inter | Inter |
| InterVariable | Inter Variable |
| Space Grotesk | Space Grotesk |
| Poppins | Poppins |
| Geist | Geist |
| SF Pro | SF Pro |
| Commit Mono | CommitMono |

Letter spacing: 0 across all font modes.

---

## Font Weight (2 modes: Default, Inverse)

| Token | Default | Inverse |
|-------|---------|---------|
| Regular | 400 | 600 |
| Medium | 500 | 500 |
| Semibold | 600 | 400 |

---

## Paragraph Spacing (2 modes: Text, Paragraph)

| Token | Text | Paragraph |
|-------|------|-----------|
| 8 | 0 | 8 |
| 10 | 0 | 10 |
| 12 | 0 | 12 |
| 14 | 0 | 14 |
| 18 | 0 | 18 |
| 24 | 0 | 24 |
| 36 | 0 | 36 |
| 48 | 0 | 48 |

---

## Typography Scale (16 text styles)

Font: Inter | Letter spacing: 0

| Style | Size | Weight | Line Height | Formula |
|-------|------|--------|-------------|---------|
| 64 Semibold | 64px | 600 | 72px | size + 8 |
| 64 Regular | 64px | 400 | 72px | size + 8 |
| 48 Semibold | 48px | 600 | 56px | size + 8 |
| 48 Regular | 48px | 400 | 56px | size + 8 |
| 32 Semibold | 32px | 600 | 40px | size + 8 |
| 32 Regular | 32px | 400 | 40px | size + 8 |
| 24 Semibold | 24px | 600 | 32px | size + 8 |
| 24 Regular | 24px | 400 | 32px | size + 8 |
| 18 Semibold | 18px | 600 | 28px | size + 10 |
| 18 Regular | 18px | 400 | 28px | size + 10 |
| 16 Semibold | 16px | 600 | 24px | size + 8 |
| 16 Regular | 16px | 400 | 24px | size + 8 |
| 14 Semibold | 14px | 600 | 20px | size + 6 |
| 14 Regular | 14px | 400 | 20px | size + 6 |
| 12 Semibold | 12px | 600 | 16px | size + 4 |
| 12 Regular | 12px | 400 | 16px | size + 4 |

**Line height rule:** lineHeight = fontSize + 8 (general), but varies slightly at small sizes.

---

## Effect Styles (10 styles)

| Style | Spec |
|-------|------|
| Glass 1 | bg-blur: 16, shadow: `rgba(0,0,0,0.04)` 0,4 blur-16 |
| Glass 2 | bg-blur: 40, shadow: `rgba(0,0,0,0.1)` 0,8 blur-28 |
| Glow | shadow: `rgba(255,255,255,0.2)` 0,0 blur-12 spread-8 + `rgba(0,0,0,0.2)` 0,0 blur-12 spread-8 |
| Focus | shadow: `rgba(0,0,0,0.04)` 0,0 blur-0 spread-4 |
| Inner shadow | 4-layer: `rgba(0,0,0,0.1)` 1,1.5 blur-4 + `rgba(0,0,0,0.08)` 1,1.5 blur-4 + `rgba(255,255,255,0.25)` 0,-0.5 blur-1 + `rgba(255,255,255,0.3)` 0,-0.5 blur-1 |
| Drop shadow 1 | `rgba(0,0,0,0.1)` 0,0.5 blur-0.5 |
| Drop shadow 2 | `rgba(0,0,0,0.1)` 0,2 blur-4 |
| Image hover | inner-shadow: `#000000` 0,20 blur-20 |
| Background blur 40 | bg-blur: 40 |
| Background blur 100 | bg-blur: 100 |

---

## Design System Limits

| Category | Max Count |
|----------|-----------|
| Text Styles | < 20 |
| Colors | < 32 |
| Effect Styles | < 8 |
| Spacing/Size/Radius | < 16 |
| Components | minimize |
