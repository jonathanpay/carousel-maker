# Carousel Maker — Custom Template Import Feature
*Session: 2026-05-30*

## What was built

A custom template import/export system was added to the carousel maker. Users can define new brand templates via JSON and import them into the tool.

### Files changed
- `templates.js` — Added CSTM generic renderer (10 layouts) + `registerCustomFamily` / `removeCustomFamily` / `getCustomFamilies` exports
- `templates.css` — Added `.cstm` family CSS using CSS custom properties for all colours
- `app.js` — Import modal logic, localStorage persistence, updated family picker
- `app.css` — Import modal and custom family card styles
- `Carousel Maker.html` — Import modal HTML

### Branch
`claude/determined-einstein-CWOr1` on `jonathanpay/carousel-maker`

---

## How to use it

In the **Template Family** section of the sidebar, there is a dashed **"+ Import template"** card at the end. Click it to open the import modal.

From the modal you can:
- **Paste JSON** directly
- **Upload a `.json` file** from disk
- **Download a sample** — a ready-to-edit starter template

Imported templates appear as family cards alongside JP and HEA. They survive page reloads (stored in `localStorage`). A **×** delete button appears on hover for custom cards.

---

## JSON schema

```json
{
  "id": "my-brand",
  "label": "My Brand — Clean",
  "subtitle": "Brief description shown under the name",
  "handle": "@mybrand",
  "logo": "https://... or data:image/png;base64,...",
  "logoStyle": "height:40px;max-width:150px;object-fit:contain",
  "fonts": {
    "headingFamily": "'Inter','Helvetica Neue',sans-serif",
    "bodyFamily": "'Inter','Helvetica Neue',sans-serif",
    "googleFonts": "Playfair+Display:wght@700,900"
  },
  "themes": {
    "dark": {
      "label": "Dark",
      "swatch": "#1a1a2e",
      "bg": "#1a1a2e",
      "bgAlt": "#16213e",
      "text": "#eaeaea",
      "textMuted": "rgba(234,234,234,0.6)",
      "accent": "#e94560",
      "accentText": "#ffffff",
      "rule": "rgba(255,255,255,0.2)"
    },
    "light": {
      "label": "Light",
      "swatch": "#ffffff",
      "bg": "#ffffff",
      "bgAlt": "#f4f4f6",
      "text": "#1a1a2e",
      "textMuted": "rgba(0,0,0,0.55)",
      "accent": "#e94560",
      "accentText": "#ffffff",
      "rule": "rgba(0,0,0,0.12)"
    }
  }
}
```

### Theme colour fields

| Field | Purpose |
|---|---|
| `bg` | Main slide background |
| `bgAlt` | Secondary/darker background (compare don't-panel, photo placeholder) |
| `text` | Primary text colour |
| `textMuted` | Secondary/caption text |
| `accent` | Highlight colour — used for eyebrows, numbers, rules, CTA pill, *italic* accent words |
| `accentText` | Text colour on top of accent backgrounds (CTA pill, compare do-panel) |
| `rule` | Decorative dividers and borders |
| `swatch` | Colour shown in the theme picker dot |

You can define as many named themes as you like.

---

## Architecture notes

### Generic renderer (.cstm)

Rather than writing 10 JavaScript renderer functions per new family, custom templates share a single generic set of renderers (`CSTM.cover`, `CSTM.stat`, etc. in `templates.js`). Each function receives the template config object and injects CSS custom properties inline on the `.slide` element:

```
--cbg, --cbg2, --ctxt, --ctxt2, --cacc, --catxt, --cbrd, --cfont-h, --cfont-b
```

The `.cstm` CSS in `templates.css` uses these properties for all styling. This means a new brand requires zero new CSS or JS — only a JSON file.

### Registration flow

```
importTemplate(json)
  → T.registerCustomFamily(cfg)         // templates.js
       → builds renderer closures (CSTM[layout] bound to cfg)
       → adds to RENDERERS[id], FAMILIES[id], THEMES[id]
       → loads Google Fonts <link> if specified
  → saveCustomTemplates()               // app.js
       → JSON.stringify(T.getCustomFamilies()) → localStorage
  → buildFamilyPicker()                 // rebuilds sidebar
  → setFamily(id)                       // activates new template
```

On page load, `loadCustomTemplates()` reads localStorage and re-registers all saved families before `buildFamilyPicker()` runs.

### Extending for a public-facing "build your own" feature

The JSON schema is the public API. Next steps to make this user-facing on a website:
1. Host a JSON editor (or form) that outputs the schema
2. The user downloads/copies the JSON
3. They paste it into the import modal — done

No server required. The tool is entirely client-side.

---

## Full conversation transcript

### User request

> One thing I would like to incorporate to the carousel maker in the ability to add new templates. The stealing of things to begin with so we have the Jonathan Pay template and we have the holistic email academy templates, I'd like to be to add to the template library. If I were to add this feature to my website as a thing that is live host and people can use initially for myself but then just being able to say okay you can build one yourself is the JSON to edit and import.

### What was explored

The codebase was explored to understand:
- 4 built-in template families (jp-editorial, jp-magazine, hea-clean, hea-shapes)
- 10 universal layouts (cover, stat, tip, list, quote, compare, case, image, author, cta)
- Each family has hardcoded JavaScript renderer functions + CSS classes
- State per slide: `{ family, layout, theme, icon, format, pg, pgTot, handle, fields }`

### Implementation decisions

**Why a generic renderer instead of copying existing families?**
Copying all 40 renderer functions for each new brand would be unmaintainable. The generic CSTM renderer uses CSS custom properties so any brand is just a color config.

**Why localStorage instead of a server?**
The tool is a static HTML file. No server = no accounts, no backend, zero hosting complexity. localStorage gives persistence without any infrastructure.

**Why inline CSS vars on the slide element?**
Injecting `--cbg:#1a1a2e` etc. directly on each `<div class="slide cstm ...">` means the styles are self-contained and travel correctly when slides are exported to PNG via html2canvas.

### Verification results

All tests passed in Playwright:
- Import modal opens/closes correctly
- Valid JSON imports and activates custom template
- Custom template renders all 10 layouts
- Theme switching works (multiple themes per template)
- localStorage persistence confirmed (template survives page reload)
- Custom template appears in showcase browser
- Invalid JSON shows error message
- Empty submit shows error message
- Showcase shows 5 rows (4 built-in + 1 custom)
