# Design — rohan roots blog

A locked design system for this site. Every page redesign reads this file before
emitting code. Do not regenerate per page — extend or amend this file when the
system needs to grow.

Stamp: `/* Hallmark · genre: editorial · macrostructure: <name> · theme: Nightshift (custom-tuned) · nav: N6 · footer: Ft4 · design-system: design.md · designed-as-app */`

## Genre
editorial

## Context (inferred from the target — redesign gate is silent)
Audience: SRE / Kubernetes / Linux practitioners. Use: read long-form essays.
Tone: plain-spoken, on-call, war-story. The design must read "printed with care",
never "generated".

## Macrostructure family
- Index pages (home, archive): **Index-First** — the page is the list. One short
  intro line, then numbered rows with hairline rules. No hero, no cards.
- Content pages (blog posts, about): **Long Document** — continuous prose,
  inline numbered section heads (S1 left-margin numbered), negative space as
  divider, typographic links as the only CTA voice.

## Theme — Nightshift (custom-tuned, not catalog)
Kept the site's existing brand: dark paper, single teal anchor. Amber demoted
out of the system entirely. Accent stays under 5% of any viewport.

- `--color-paper`    oklch(0.162 0.0143 258.4)   #0a0e14
- `--color-paper-2`  oklch(0.199 0.0198 262.0)   #11161f
- `--color-ink`      oklch(0.940 0.0083 271.3)   #e9ebf1
- `--color-ink-2`    oklch(0.676 0.0267 265.5)   #8f97a8  (muted)
- `--color-rule`     oklch(0.292 0.0303 260.5)   #232c3b  (hairlines)
- `--color-accent`   oklch(0.785 0.1325 181.9)   #2dd4bf  (teal, the one anchor)
- `--color-accent-ink` oklch(0.272 0.0447 183.0) #052e29 (ink on accent fills)
- `--color-focus`    oklch(0.785 0.1325 181.9)   (same as accent)

## Typography
- Display: **Fraunces**, roman, 600. Tight tracking (-0.02em). Never italic.
- Body: **Newsreader**, 400 (italic allowed for emphasis in long-form only).
- Mono (technical register — code, dates, colophon): **JetBrains Mono**, 400/500.
- Three families is the ceiling. Mono never does body work.
- Type scale anchor: `--text-display: clamp(2.75rem, 5vw + 1rem, 4.5rem)`.
  Ratio 1.25. Body 1.0625rem, line-height 1.65, measure 60–65ch.

## Spacing
4-point named scale (lives in `src/styles/tokens.css`; pages use named tokens,
never raw values):
`--space-3xs: 0.25rem · --space-2xs: 0.5rem · --space-xs: 0.75rem ·
--space-sm: 1rem · --space-md: 1.5rem · --space-lg: 2rem · --space-xl: 3rem ·
--space-2xl: 4.5rem · --space-3xl: 7rem`

Page gutter: `clamp(1.25rem, 5vw, 3rem)`.

## Motion
- None. The page is just there (Long Document / Index-First reveal: none).
- Microinteractions: link underline slide + colour shift only.
- Easings (for the hover transitions): `--ease-out: cubic-bezier(0.16, 1, 0.3, 1)`.
- `prefers-reduced-motion` respected everywhere.

## Microinteractions stance
- Silent. No toasts, no reveals, no scroll choreography.
- Hover: 1px underline slides in; colour shifts to accent on index titles.
- Focus: visible accent outline, 0 delay.

## CTA voice
- **C3 typographic link** — a word, an arrow, a 1px underline. No buttons on
  this site. ("Read →", "RSS →".)

## Navigation — N6 Newspaper masthead
Knobs: issue-line=above wordmark (his tagline, serif small caps) ·
wordmark=2xl lowercase Fraunces · rule=double.
Centered wordmark is the archetype (broadsheet), not default-centering.
Active page gets `aria-current` + accent underline.

## Footer — Ft4 Dense typographic colophon
Knobs: family=monospace · layout=single block · includes=set-in line, build
credits, RSS. Ragged-right, muted. No link farm — Elsewhere lives on /about.

## Per-page allowances
- Index pages: numbered rows, hairline rules, mono dates. No images.
- Content pages: typography only. Cover images (when a post ships one) get a
  hairline frame, sized to the text measure, never full-bleed.
- No enrichment anywhere. No card borders, no shadows, no gradients.

## What pages MUST share
- The wordmark (`rohan roots`, lowercase Fraunces).
- The single teal accent and its placement (numbers, rules-adjacent, links).
- The three typefaces.
- The C3 link voice.
- Hairline rules as the only divider language.

## What pages MAY differ on
- Macrostructure within the family (Index-First vs Long Document).
- Container alignment: index pages full-gutter grid; content pages left-biased
  65ch column (`margin-left: clamp(1.25rem, 8vw, 10rem)` on desktop).
- About may open with a display statement; articles open with kicker + title.

## Exports

### tokens.css
```css
:root {
  --color-paper:      oklch(0.162 0.0143 258.4);
  --color-paper-2:    oklch(0.199 0.0198 262.0);
  --color-ink:        oklch(0.940 0.0083 271.3);
  --color-ink-2:      oklch(0.676 0.0267 265.5);
  --color-rule:       oklch(0.292 0.0303 260.5);
  --color-accent:     oklch(0.785 0.1325 181.9);
  --color-accent-ink: oklch(0.272 0.0447 183.0);
  --color-focus:      oklch(0.785 0.1325 181.9);

  --font-display: "Fraunces", ui-serif, Georgia, serif;
  --font-body: "Newsreader", ui-serif, Georgia, serif;
  --font-mono: "JetBrains Mono", ui-monospace, SFMono-Regular, monospace;

  --space-3xs: 0.25rem;  --space-2xs: 0.5rem;  --space-xs: 0.75rem;
  --space-sm: 1rem;      --space-md: 1.5rem;   --space-lg: 2rem;
  --space-xl: 3rem;      --space-2xl: 4.5rem;  --space-3xl: 7rem;

  --text-xs: 0.75rem;  --text-sm: 0.875rem; --text-base: 1rem;
  --text-md: 1.0625rem; --text-lg: 1.25rem; --text-xl: 1.5625rem;
  --text-2xl: 1.953rem; --text-3xl: 2.441rem;
  --text-display: clamp(2.75rem, 5vw + 1rem, 4.5rem);

  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --dur-short: 220ms;
  --gutter: clamp(1.25rem, 5vw, 3rem);
  --measure: 65ch;
}
```

### Tailwind v4 @theme
```css
@theme {
  --color-paper: oklch(0.162 0.0143 258.4);
  --color-paper-2: oklch(0.199 0.0198 262.0);
  --color-ink: oklch(0.940 0.0083 271.3);
  --color-ink-2: oklch(0.676 0.0267 265.5);
  --color-rule: oklch(0.292 0.0303 260.5);
  --color-accent: oklch(0.785 0.1325 181.9);
  --font-display: "Fraunces", ui-serif, Georgia, serif;
  --font-body: "Newsreader", ui-serif, Georgia, serif;
  --font-mono: "JetBrains Mono", ui-monospace, monospace;
}
```

### DTCG tokens.json
```json
{
  "color": {
    "paper":   { "$value": "oklch(0.162 0.0143 258.4)", "$type": "color" },
    "paper-2": { "$value": "oklch(0.199 0.0198 262.0)", "$type": "color" },
    "ink":     { "$value": "oklch(0.940 0.0083 271.3)",  "$type": "color" },
    "ink-2":   { "$value": "oklch(0.676 0.0267 265.5)",  "$type": "color" },
    "rule":    { "$value": "oklch(0.292 0.0303 260.5)",  "$type": "color" },
    "accent":  { "$value": "oklch(0.785 0.1325 181.9)",   "$type": "color" }
  },
  "font": {
    "display": { "$value": "Fraunces", "$type": "fontFamily" },
    "body":    { "$value": "Newsreader", "$type": "fontFamily" },
    "mono":    { "$value": "JetBrains Mono", "$type": "fontFamily" }
  }
}
```

### shadcn/ui CSS variables
```css
:root {
  --background: 0.162 0.0143 258.4;
  --foreground: 0.940 0.0083 271.3;
  --primary: 0.785 0.1325 181.9;
  --primary-foreground: 0.272 0.0447 183.0;
  --muted: 0.292 0.0303 260.5;
  --muted-foreground: 0.676 0.0267 265.5;
  --border: 0.292 0.0303 260.5;
  --ring: 0.785 0.1325 181.9;
  --radius: 0.25rem;
}
```
