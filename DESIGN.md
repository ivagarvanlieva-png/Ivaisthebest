# Design System — iService

The visual identity for iService, derived from the landing pages. Hand this to Lovable alongside CLAUDE.md and PRD.md so the app is built in this look, not the AI's default.

## Design principles

- Service-oriented and practical. The design gets out of the way so people get things done.
- Editorial, not techy. A serif display face gives it warmth and confidence; the rest is quiet.
- Warm neutrals, one bold accent. Spend boldness on the burnt-orange accent; keep everything around it calm.
- Avoid the generic AI-app look: no purple-to-blue gradients, no everything-rounded cards, no emoji as icons, no centered-everything.

## Color palette

| Token | Hex | Use |
|-------|-----|-----|
| `--white` | `#FAFAF8` | Page background (warm near-white, not pure white) |
| `--ink` | `#0D0C0B` | Primary text, dark sections (warm near-black) |
| `--accent` | `#C8410A` | Burnt orange — primary buttons, links, active state, brand `i` |
| `--accent-dark` | `#A33009` | Accent hover/pressed |
| `--muted` | `#7C7874` | Secondary text, captions, labels |
| `--surface` | `#EDEAE3` | Section backgrounds, cards, fills |
| `--rule` | `#DAD6CF` | Borders, dividers, input outlines |

Semantic colors for job status (separate from the accent):
- open = `--muted` grey, accepted = `#2563B0` blue, in progress = `#B5820A` amber, completed = `#3C7A3C` green.

## Typography

- **Display / headings:** Georgia, serif. Large, tight letter-spacing (`-0.02em` to `-0.03em`), `text-wrap: balance`. Used for the wordmark, page titles, card headings.
- **Body / UI:** system-ui sans-serif stack. Used for all running text, forms, buttons, data.
- **Labels / eyebrows:** sans, uppercase, `0.13em–0.16em` letter-spacing, small (`~0.7rem`), in `--muted` or `--accent`.
- Keep running text near 65 characters wide. Set a type scale and stay on it.

## Brand mark

`iService` set in Georgia. The leading **`i`** is in `--accent`; the rest is `--ink`. Never restyle per page.

## Components

- **Buttons:** rectangular, not pill-shaped. Primary = `--accent` fill, white text, uppercase, letter-spaced; hover → `--accent-dark`. Secondary = transparent with a `--rule` border that darkens to `--ink` on hover. Visible focus ring (`2px` accent, `3px` offset).
- **Inputs:** `1.5px` `--rule` border on `--white`; border turns `--accent` on focus. Square corners.
- **Cards:** flat, bordered with `--rule` or filled `--surface`. Minimal or no border-radius (≤ 2px). No drop shadows by default.
- **Status pills:** small, uppercase, color-coded per the semantic table above; tinted background with darker text.
- **Chips / tags** (category, district): small uppercase label in a thin `--rule` border.
- **Dividers:** 1px `--rule` hairlines.

## Layout

- Centered container, max-width ~1100–1140px, generous side padding (`clamp(1.25rem, 5vw, 3rem)`).
- Lay out groups with flex/grid and `gap`, not stacked margins.
- Wide content (tables, message threads) scrolls inside its own container — the page body never scrolls sideways.
- Dark `--ink` sections are used sparingly for contrast (e.g. a categories band), with `--accent` as the only color pop.

## Motion

- Restraint. Short transitions on hover/focus (~0.15s) for buttons, links, inputs.
- No elaborate scroll animations. Respect `prefers-reduced-motion`.

## Voice in UI copy

- Active voice; a control says exactly what happens: "Post job", then a confirmation "Job posted".
- Errors say what went wrong and how to fix it — no apologies, no vagueness.
- Specific over clever. "Someone in Madrid can do that." over taglines.

## Lovable / shadcn mapping

Lovable ships shadcn/ui + Tailwind. Map this system onto it rather than fighting it:
- Set Tailwind theme colors to the tokens above; make `--accent` the primary.
- Override the default `border-radius` to near-zero (shadcn defaults to rounded — square it off).
- Set the display font to a serif (Georgia) for headings via Tailwind `font-serif`; keep `font-sans` as system-ui for body.
- Use Card, Badge (status pills), Dialog, Tabs, Form, Input, Button from shadcn — restyled with these tokens, not their defaults.
