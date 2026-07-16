---
name: brand-guidelines
description: Applies Anthropic's official brand colors and typography to any sort of artifact that may benefit from having Anthropic's look-and-feel. Use it when brand colors or style guidelines, visual formatting, or company design standards apply.
license: Complete terms in LICENSE.txt
---

# Anthropic Brand Styling

The brand look is warm, calm, and editorial: soft warm neutrals, one confident accent,
a serif body voice. Most brand failures are not wrong hex codes — they are right hex
codes in the wrong roles. This file tells you which color does which job.

## The palette (exact values — never approximate or "improve")

**Main colors:**

- Dark: `#141413`
- Light: `#faf9f5`
- Mid Gray: `#b0aea5`
- Light Gray: `#e8e6dc`

**Accent colors:**

- Orange: `#d97757` — primary accent
- Blue: `#6a9bcc` — secondary accent
- Green: `#788c5d` — tertiary accent

## Color roles

| Role | Light artifact | Dark artifact |
|---|---|---|
| Background | `#faf9f5` | `#141413` |
| All text (headings + body) | `#141413` | `#faf9f5` |
| Cards, panels, subtle zones | `#e8e6dc` | lighten `#141413` slightly (e.g. `#232320`) |
| Hairlines, borders, gridlines | `#e8e6dc` (light) / `#b0aea5` (stronger) | `#b0aea5` at low width |
| Metadata, captions, axis labels | `#b0aea5` — small text only | `#b0aea5` |
| Emphasis, rules, chart series, icons | accents, orange first | accents, orange first |

Judgment calls the table can't carry:

- **Text is always the Dark/Light pair.** That pairing is ~15:1 contrast and is the
  only pair rated for paragraphs. Never set body text in an accent color or in Mid
  Gray — Mid Gray on Light is roughly 2:1 and fails readability at body sizes. Mid
  Gray is legitimate only for short, small, skippable text: captions, timestamps,
  axis ticks.
- **Accents are ~3:1 against both backgrounds** — fine for shapes, fills, chart
  series, large display numbers, and rules; not fine for sentences. If a whole
  sentence needs emphasis, make it Dark and bold, don't make it orange.
- **Never pure black `#000` or pure white `#fff`.** The warmth of `#141413`/`#faf9f5`
  *is* the brand. Pure black/white next to them reads as a bug.

## Accent discipline

- **Default: one accent per artifact, and it's orange.** A page with one orange
  gesture looks branded; a page with all three accents looks like a template.
- Blue and green exist for when orange alone can't do the job:
  - **Categorical data series**: cycle orange → blue → green, in that order.
  - **Paired states** (before/after, A/B): orange for the one you want the eye on,
    blue for the comparison.
- Never use accents as large background fills for text-bearing regions. An orange
  callout box with dark text on it is off-brand; a Light Gray box with an orange left
  rule is on-brand.
- Semantic caution: green is a palette color, not a "success" light, and there is no
  red. For error/warning states, use orange plus wording, not an imported red.

## Typography

- **Headings (24pt and larger): Poppins** — fall back to Arial if unavailable.
- **Body text: Lora** — fall back to Georgia if unavailable.
- Fonts should be pre-installed in your environment for best results; the fallbacks
  preserve the sans-heading/serif-body structure, which matters more than the exact
  faces.

Judgment calls:

- The heading/body contrast (geometric sans over warm serif) is the typographic
  brand. Never set body paragraphs in Poppins, and never set a headline in Lora.
- Small utility text (chart labels, table headers, captions, UI chrome) may use
  Poppins/Arial at small sizes — serif at 9pt gets muddy.
- Weights: Poppins at Medium/SemiBold for headings — Bold everywhere shouts, Light
  everywhere disappears. Lora body at Regular; use *italic* for emphasis within body
  text before reaching for color.
- Don't letterspace Lora; modest letterspacing on short all-caps Poppins labels
  (eyebrows, chart headers) is fine.

## Worked examples

**A slide:** Light background `#faf9f5`. Title in Poppins SemiBold `#141413`. A single
orange rule or accent block anchoring the title area. Body bullets in Lora `#141413`.
Footer/page number in `#b0aea5`. Any content card sits on `#e8e6dc` with no border.
That's the whole recipe — if a slide has more color events than that, remove some.

**A bar chart:** Plot background same as page (`#faf9f5`, not white). Bars: series 1
orange, series 2 blue, series 3 green — more than three series means the chart should
probably be redesigned, not the palette extended. Gridlines `#e8e6dc`, drawn behind.
Axis labels and ticks `#b0aea5`, small. Title in Poppins `#141413`; a highlighted
data label may be orange, everything else dark.

**An HTML page or artifact:**

```css
:root {
  --bg: #faf9f5; --ink: #141413;
  --panel: #e8e6dc; --muted: #b0aea5;
  --accent: #d97757; --accent-2: #6a9bcc; --accent-3: #788c5d;
}
body { background: var(--bg); color: var(--ink);
       font-family: Lora, Georgia, serif; }
h1, h2, h3 { font-family: Poppins, Arial, sans-serif; }
```

Dark mode: swap `--bg` and `--ink`; accents stay the same; panels become a slight
lighten of Dark rather than Light Gray.

**python-pptx:** apply via `RGBColor` — e.g. `RGBColor(0x14, 0x14, 0x13)` for text,
`RGBColor(0xD9, 0x77, 0x57)` for the accent shape fill. Headings 24pt+ get Poppins,
body gets Lora, matching the role table above.

## What reads as off-brand (reject these on sight)

- Purple, teal, neon, or gradient fills of any kind
- Pure `#000`/`#fff`, or cool grays (`#888`, `#ccc`) instead of the warm neutrals
- All three accents decorating one composition without a data reason
- Orange body text, or Mid Gray paragraphs
- Heavy borders and drop shadows — the brand separates regions with the Light
  Gray/Light contrast and whitespace, not outlines

## Final check before delivering

Squint at the artifact: you should see warm paper, dark ink, and one orange moment.
Verify every paragraph is the Dark/Light pair, every accent is doing a job (pointing
at something), and the fonts split sans-headings/serif-body. If any element could be
deleted without losing information, the brand look improves — delete it.
