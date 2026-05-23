# Carousel Maker

A browser-based tool for designing and exporting social media carousel slides for two brands: **Jonathan Pay (JP)** and **Holistic Email Academy (HEA)**.

No build step, no dependencies to install — open an HTML file and start designing.

---

## Files

| File | Purpose |
|---|---|
| `Carousel Maker.html` | Main entry point — loads `app.js`, `templates.js`, and CSS separately |
| `Carousel Maker-standalone.html` | Self-contained version using bundled JS (no separate script files needed) |
| `Carousel Maker-print.html` | Print-optimised layout for generating PDFs |
| `app.js` / `app-bundled.js` | App logic (state, sidebar, export, tweaks) |
| `templates.js` / `templates-bundled.js` | Template render functions and slide definitions |
| `app.css` | UI chrome styles (sidebar, toolbar, tray) |
| `templates.css` | Slide visual styles |

## Usage

Open `Carousel Maker.html` (or the standalone variant) directly in a browser. No server required.

---

## Template Families

Four design families cover both brands:

| Key | Label | Style |
|---|---|---|
| `jp-editorial` | JP — Editorial | Refined navy + gold |
| `jp-magazine` | JP — Magazine | Luxe oversized italic |
| `hea-clean` | HEA — Clean | Bright purple, polished |
| `hea-shapes` | HEA — Shapes | Dark + geometric blocks |

Each family has its own colour themes (e.g. navy, white, gold for JP Editorial; purple, cyan, green, dark for HEA Clean).

## Layouts

Ten slide layouts are available across all families:

| Key | Label | Use for |
|---|---|---|
| `cover` | Cover | Series opener with eyebrow, headline, subheadline |
| `stat` | Big Stat | Single headline statistic with supporting context |
| `tip` | Numbered Tip | Step-by-step tips with kicker and body |
| `list` | Checklist | Up to 6 bullet items |
| `quote` | Pull Quote | Large pull quote with attribution |
| `compare` | Compare | Side-by-side Don't / Do contrast |
| `case` | Case Study | Problem → action → result narrative |
| `image` | Image + Caption | Full-bleed photo with caption |
| `author` | Author | Bio card with photo, name, and role |
| `cta` | CTA | Call-to-action closer slide |

## Formats

Slides can be toggled between **Square** and **Portrait** using the toolbar buttons.

---

## Key Features

**Multi-slide deck** — Add, duplicate, and reorder slides in the filmstrip tray at the bottom. Each slide holds its own layout, family, theme, and content independently.

**Live preview** — The canvas updates instantly as you type or change settings in the sidebar.

**Accent colour syntax** — Wrap any word(s) in `*asterisks*` in headline/body fields to render them in the theme's accent colour. Example: `5 *tips* to write better emails`.

**Tweaks panel** — Global settings accessible via the ⚙ Tweaks button:
- Show/hide page numbers in the footer
- Show/hide handles (username) in the footer
- Show/hide watermark numbers on Tip slides
- Font scale (80–130%)
- Outer padding scale (60–140%)

**Template browser** — The ⊞ Browse templates button opens a full library showing all 4 families × 10 layouts, so you can insert any slide into your deck with one click.

**PNG export** — Export the current slide (↓ PNG) or all slides in one go (↓ Export all) using html2canvas.

**Icon picker** — HEA templates support decorative icons on Tip, Quote, and Cover slides, drawn from the `assets/icons/` directory.

---

## Assets

- `assets/icons/` — Icon PNGs used by HEA templates
- `assets/logos/` — Brand logos
- `uploads/` — User-uploaded images and brand guideline documents

---

## Dependencies

- [html2canvas 1.4.1](https://html2canvas.hertzen.com/) — loaded via CDN for PNG export. No other runtime dependencies.
