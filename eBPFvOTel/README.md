# eBPF + OTel — live workshop deck

Slidev deck for **"eBPF + OTel: Two Layers, One Observability Strategy"**
(David Flanagan + Anais Dotis-Georgiou, September 2, 2026).

Styling comes from the groundcover brand template, extracted from the
*groundcover overview* pptx.

## Run it

```bash
npm install      # first time only
npm run dev      # http://localhost:3030 — hot reloads as you edit slides.md
npm run build    # static site into dist/
npm run export   # PDF (needs: npx playwright install chromium)
```

Presenter view is at `http://localhost:3030/presenter` — speaker notes show there.

---

## What's in here

```
slides.md            ← THE DECK. This is the only file you normally edit.
public/              images, served at the URL root
  logo.png             groundcover wordmark (used by every layout)
  hero.png             title-slide artwork
  david.jpg            speaker headshot
  anais.jpg            speaker headshot
styles/index.css     brand tokens — colors, type scale, shared component CSS
style.css            one line; just imports styles/index.css (Slidev looks for this name)
layouts/             page frames you pick with `layout:` in a slide's frontmatter
  gc.vue               standard slide — logo, section label, NN/NN counter, title, subhead
  gc-hero.vue          opening slide — big headline, subhead, logo row, artwork
  gc-statement.vue     one big line, optional CTA (closing or section break)
components/          reusable blocks you drop into a slide's body
  Steps.vue            3-stage numbered progression, gold gradient
  Cards.vue            3 tinted cards (green → gold → blue)
  Compare.vue          comparison table, one column highlighted green   (unused today)
  Stats.vue            4 stat tiles + an outcome pill                   (unused today)
package.json         scripts + the two dependencies
node_modules/        installed packages — never edit, never commit
```

`Compare.vue` and `Stats.vue` aren't used by the current six slides. They're kept
because they're the fastest way to add a results slide or a comparison table later.

---

## The deck, slide by slide

| # | Layout | What it does |
|---|---|---|
| 1 | `gc-hero` | Title, the "neither layer replaces the other" framing, workshop date |
| 2 | `gc` + inline HTML | Speaker cards with headshots |
| 3 | `gc` + `<Steps>` | Agenda as Deploy → Compare → Prove, with the three examples named below |
| 4 | `gc` + inline HTML | eBPF vs OTel, two tinted columns, takeaway pill |
| 5 | `gc` + inline HTML/SVG | The BYOC architecture diagram + yellow value band |
| 6 | `gc` + `<Cards>` | The three demo examples: what exists, what we'd build |

---

## Editing slides.md

Slides are separated by `---` on its own line. Everything between the first pair
of `---` on a slide is **frontmatter** (YAML), everything after is the body
(Markdown + HTML + Vue components).

```markdown
---
layout: gc
eyebrow: Concepts          # small label, top right
title: eBPF vs OTel        # big headline
sub: One line under it.    # optional
---

body goes here

<!--
Speaker notes. Last comment block on a slide becomes the notes in presenter view.
-->
```

The frontmatter fields each layout accepts:

- **`gc`** — `eyebrow`, `title`, `sub` (all optional; omit `title` to get a full-bleed body)
- **`gc-hero`** — `headline`, `sub`, `logos` (list of strings), `art` (image path)
- **`gc-statement`** — `headline`, `sub`, `cta`

`headline` and `sub` render as HTML, so `<br/>` controls line breaks and
`<span class="gc-hl-g">…</span>` highlights a phrase in green (`gc-hl-y` for gold).

### Using the components

```markdown
<Steps :items="[
  { title: 'Deploy',  detail: 'What this stage does.' },
  { title: 'Compare', detail: '…' },
  { title: 'Prove',   detail: '…' },
]" />

<Cards :items="[
  { icon: '🐢', title: 'Card one', points: ['First point.', 'Second point.'] },
  { icon: '🗄', title: 'Card two', points: ['…'] },
  { icon: '🔊', title: 'Card three', points: ['…'] },
]" />
```

Card tints cycle green → gold → blue, so put the strongest one first.
Points render as HTML — `<b>`, `<code>` and `<br/>` all work inside them.

### Icons

Any [Carbon](https://carbondesignsystem.com/elements/icons/library/) or
[Phosphor](https://phosphoricons.com/) icon works inline, no import needed:

```html
<carbon:document />  <carbon:kubernetes />  <ph:cloud />
```

If an icon name doesn't exist the build fails with a resolve error — check the
name in the library first.

### Brand tokens

Defined at the top of `styles/index.css`, usable as `var(--gc-…)` anywhere:

```
--gc-bg      #F7F3F0   warm cream page       --gc-yellow      #F6C73C
--gc-bg-3    #F3EEEA   tinted panel          --gc-yellow-deep #EAB20B
--gc-rule    #E3DCD9   hairline borders      --gc-yellow-pale #FBE9B1
--gc-ink     #181818   body text             --gc-green       #00A47B
--gc-navy    #1E3048                         --gc-blue        #80A0FF
--gc-slate   #384653   secondary text        --gc-tint-green  #E8F5F0
--gc-muted   #8C9BAA   labels, counters      --gc-tint-blue   #EDF0FE
```

Handy shared classes: `.gc-pill--dark` (black rounded takeaway pill),
`.gc-pill` (gold block), `.gc-eyebrow` (small uppercase label),
`.gc-card` + `.gc-card--green|yellow|blue` (tinted panel), `.gc-hl-g` / `.gc-hl-y`.

Tailwind-style utilities (`mt-6`, `grid`, `grid-cols-3`, `gap-4`, `text-center`)
work too — Slidev ships UnoCSS.

---

## Four gotchas that will bite you

These are real failure modes, not theory — each one broke this deck at least once.

**1. No blank lines inside a block of HTML.** Markdown ends the HTML block at the
first blank line, and the next indented line becomes a *code block* — your slide
renders as literal `<div class="…">` source. If a slide suddenly shows its own
markup, this is why. Keep HTML contiguous.

**2. But you DO need a blank line before `<style>`.** Without it the style tag
gets swallowed into the Vue template and the build fails with
*"Tags with side effect are ignored in client component templates."*

**3. `<style>` blocks are scoped — they don't reach inside components.** A rule
like `.gc-cards .gc-card__title { font-size: 14px }` silently does nothing,
because the card internals belong to `Cards.vue`, not to your slide. Wrap the
inner part in `:deep()`:

```css
.gc-cards :deep(.gc-card__title) { font-size: 14px; }
```

**4. The canvas is 980 × 551 px.** Font sizes in `styles/index.css` are tuned to
that, so a `12px` rule really is small. Content that overflows just runs off the
bottom of the slide — no scrollbar, no warning. Check the bottom edge after
adding text.

---

## Changing the deck size or fonts

Both live in the frontmatter of the **first** slide in `slides.md`:

```yaml
aspectRatio: 16/9
canvasWidth: 980        # every px value in the CSS is relative to this
fonts:
  sans: Inter
  serif: Space Grotesk
  weights: '300,400,500,600,700,800'
  provider: google
```

Raising `canvasWidth` makes all text proportionally smaller relative to the
slide, so change it only if you're prepared to retune the type scale.
