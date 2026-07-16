---
name: theme-factory
description: Toolkit for styling artifacts with a theme. These artifacts can be slides, docs, reportings, HTML landing pages, etc. There are 10 pre-set themes with colors/fonts that you can apply to any artifact that has been creating, or can generate a new theme on-the-fly.
license: Complete terms in LICENSE.txt
---

# Theme Factory Skill

Apply one of 10 curated font/color themes — or a custom theme generated on the fly —
to any artifact: slide decks, docs, reports, HTML landing pages.

A theme file gives you four colors and two fonts. It does **not** tell you which color
paints which surface — that mapping is where themed artifacts succeed or fail, and it
is defined below. Follow it for every theme, preset or custom.

## Workflow

1. **Show the theme showcase**: display `theme-showcase.pdf` so the user can see all
   themes visually. Don't modify it; just show it.
2. **Ask for their choice** — if the content has an obvious fit, recommend one
   ("Ocean Depths suits a finance review; Botanical Garden suits your garden-party
   invite") but let them decide.
3. **Wait for explicit selection.**
4. **Apply the theme** using the role mapping below.

## Themes available

Each is defined in `themes/` and showcased in `theme-showcase.pdf`:

1. **Ocean Depths** - Professional and calming maritime theme
2. **Sunset Boulevard** - Warm and vibrant sunset colors
3. **Forest Canopy** - Natural and grounded earth tones
4. **Modern Minimalist** - Clean and contemporary grayscale
5. **Golden Hour** - Rich and warm autumnal palette
6. **Arctic Frost** - Cool and crisp winter-inspired theme
7. **Desert Rose** - Soft and sophisticated dusty tones
8. **Tech Innovation** - Bold and modern tech aesthetic
9. **Botanical Garden** - Fresh and organic garden colors
10. **Midnight Galaxy** - Dramatic and cosmic deep tones

## Applying a theme: the role mapping

Read the chosen theme file from `themes/`. Its palette lists ~4 colors with usage
hints. Assign them to four roles before touching the artifact:

- **Background** — the lightest color (or darkest, for a deliberately dark theme).
  Covers most of every page. If the palette's lightest color is still strong (e.g. a
  saturated blue), use a near-white tint of it instead and keep the strong version
  for panels.
- **Ink** — body and heading text. Must contrast with Background at roughly 4.5:1 or
  better for body text. If no palette color qualifies, use near-black `#1a1a1a` (or
  near-white on dark themes) — readable text outranks palette purity, always.
- **Primary accent** — the palette's most characteristic color: headings or heading
  underlines, chart series 1, links, callout rules, the one thing per page that
  should catch the eye.
- **Secondary accent** — supporting color: chart series 2, subtle panel fills (tint
  it toward the background), icons, dividers.

Rules that hold for every theme:

- **Accent colors never set body text.** Accents decorate and point; Ink reads.
  A themed deck with teal paragraphs is the classic failure — don't produce it.
- **Ration the accents.** Background and Ink do ~90% of the work; if every element
  is colorful, the theme reads as clipart. One primary-accent event per slide/section.
- **Large text (titles ≥24pt) may use an accent** if it clears ~3:1 against the
  background; body text may not.
- **Charts**: series colors from the accents in palette order; gridlines a light
  tint of Ink; labels in Ink. Never rainbow beyond the palette.
- **Contrast check is mandatory**, not aspirational: for each text/background pair
  you actually used, sanity-check it (mentally or with a quick script). Any pair you
  squint at fails — swap to Ink.

### Worked example — Ocean Depths on a slide deck

Palette: deep navy, ocean blue, seafoam, sand/cream. Mapping: Background = sand/cream
· Ink = deep navy · Primary accent = ocean blue · Secondary = seafoam.

- Title slide: cream background, title in the theme's header font in deep navy, one
  ocean-blue rule under the title, presenter/date line in navy at 60% size.
- Content slide: heading navy with an ocean-blue underline accent; body bullets navy;
  a highlight stat in ocean blue at display size; panel behind a quote in seafoam
  tinted toward cream (not full-strength seafoam).
- Chart: series 1 ocean blue, series 2 seafoam, axis text navy, gridlines pale navy.
- What you never see: seafoam body text, navy background with navy text, all four
  colors on one slide.

## Typography

Theme files name header/body fonts (often DejaVu — a safe cross-platform default,
not a design statement). Keep each font in its lane: header font for headings only,
body font for running text. If the environment has a higher-quality font matching
the theme's spirit (a humanist serif for organic themes, a geometric sans for tech
themes), you may substitute it — preserve the header/body *pairing structure* and say
what you substituted. When in doubt, ship the theme file's fonts.

## Create your own theme

When none of the 10 fit, generate a custom theme in the same format:

- 4 colors that satisfy the four roles above — in particular one workable
  Background and one Ink with real contrast between them. Build the palette around
  the subject (a vineyard invite earns wine + cream; a robotics pitch earns graphite
  + signal color), not around generic "professional blue."
- A header/body font pairing with a reason ("slab serif for weight, humanist sans
  for body").
- A name in the style of the existing ten, describing what the combination evokes.

Show the generated theme for review and verification before applying it, then apply
it through the same role mapping and rules as a preset theme.

## Final check

Flip through the finished artifact once: every page shows Background + Ink doing the
work with accents pointing at what matters; no text pair fails the squint test; fonts
stay in their lanes; the theme is recognizable from any single page. If a page looks
busier than the showcase page for that theme, pull color out until it doesn't.
