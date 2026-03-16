# 🌿 VANA — Vegetation & Avian Native Atlas
### v1.0 · Location-based biodiversity intelligence for landscape architects

Built by **Greenscape Designz** — a design-intelligence tool that converts regional biodiversity data into actionable landscape guidance: native planting lists, soil specifications, irrigation demand, maintenance calendars, and measurable ecological impact metrics.

---

## 📁 Complete file structure

```
vana/
│
├── index.html                         ← Main application (all UI, logic, styles)
│
├── README.md                          ← This file
│
├── data/
│   ├── species.json                   ← Bird species database (9 nectarivores + guilds)
│   └── plants.json                    ← Native plant database (nectar + canopy species)
│
└── .github/
    └── workflows/
        └── deploy.yml                 ← Auto-deploy to GitHub Pages on every push
```

**Total files: 5**
Everything the app needs to run is contained in these 5 files. There are no build steps, no npm install, no dependencies. It runs as plain HTML in any browser.

---

## 🗂 What each file does

### `index.html` (main app — 904 lines)
The entire VANA application lives in this single file. It contains:
- All HTML structure for 7 tabs
- All CSS styles (embedded in `<style>` block)
- All JavaScript logic (embedded in `<script>` block)
- All ecological data arrays (species, plants, maintenance, calendar, impact)
- All interactive behaviour — drawers, toggles, toasts, navigation

This deliberate single-file architecture means:
- No server required — open it directly in a browser
- No build process — edit and save
- Easy to deploy anywhere — just one file to upload
- Easy to share — send one file to a colleague

### `data/species.json`
Complete species records for 9 nectarivore birds including:
- Ecological guild, habitat layer, nesting type
- Feeding style, breeding season, observation frequency
- Key attractant plants
- Design tips specific to South Indian farmhouse landscapes
- Stub arrays for frugivores, insectivores, raptors, waterbirds (ready for V2 expansion)

### `data/plants.json`
Plant records for all species in the planting list including:
- Habitat layer, blooming season, water demand
- Soil pH range, spacing, sunlight requirements
- Bird species attracted
- Planting and care notes for Bengaluru / South India conditions
- Separate arrays: `nectar_plants` and `canopy_trees`

### `.github/workflows/deploy.yml`
GitHub Actions workflow that:
- Triggers automatically on every push to the `main` branch
- Deploys the entire repository to GitHub Pages
- Takes approximately 60–90 seconds to complete
- Provides a live public URL at `https://YOUR_USERNAME.github.io/vana/`
- No configuration needed — it works as-is

---

## 📱 App tabs — full feature map

| Tab | What it contains | All interactions |
|-----|-----------------|-----------------|
| **Scan** | GPS location, project context selector, site size slider, data source status | Context buttons toggle selection + toast feedback. Scan button runs 1.8s loading animation then navigates to Birds tab |
| **Birds** | 4 metric cards, 7 ecological guild rows, seasonal activity grid | Metric cards show tooltips on tap. Nectarivores + Frugivores navigate to their dedicated tabs. Insectivores, Water birds, Raptors, Ground birds, Cavity nesters each open inline species drawers with design guidance |
| **Nectarivores** | 4 sub-tabs: Species guide, Nectar plants, Garden design, Seasonal calendar | Species cards expand full profile drawers with attributes, plant chips, and design tips. Plant chips cross-link to Plants tab. Each design zone expands with implementation detail. All 12 calendar months open detail panels |
| **Plants** | 4 metric cards, 12-plant native planting list with layer/blooming/priority | Every plant row expands a full care drawer: water demand, soil pH, spacing, sunlight, birds attracted, care notes |
| **Soil & Water** | Red laterite soil profile, 5 amendment bars, 4 seasonal irrigation bars | All 4 soil profile cards are tappable with tooltips. All amendment and irrigation bar rows are tappable with specific guidance |
| **Maintenance** | 12-month calendar grid, breeding season warning | All 12 months open full 4-task checklists in a detail panel below the grid |
| **Impact** | 4 metric cards, 7 ecological outcome rows, client-ready statement | Metric cards show tooltips. All 7 impact rows expand with full mechanism + timeline explanation |

---

## 🔧 How to customise

### Adding a new bird species
Open `index.html` and find the `const SP=[` array in the `<script>` block. Add a new object:

```javascript
{
  id:'s10',           // unique id — increment from last
  i:'🟡',            // emoji icon displayed on card
  bg:'#fef8e6',      // background colour for detail icon circle
  n:'New species',   // common name
  sc:'Scientific name',
  st:'Year-round',   // status shown as pill: 'Year-round' / 'Resident' / 'Monsoon visitor' / 'Rare'
  sc2:'pg',          // pill colour class: pg=green, pt=terra, pa=amber, pp=purple, pb=blue
  len:'12 cm',
  ly:'Shrub layer',
  ne:'Cup nest',
  br:'Feb – May',
  fe:'Perch-feed',
  fr:'High',
  de:'Species description — 2–3 sentences on ecology and behaviour.',
  pl:['Plant 1','Plant 2','Plant 3'],  // key attractant plants
  ti:[
    'Design tip 1 specific to South Indian farmhouse landscapes',
    'Design tip 2'
  ]
}
```

### Adding a new plant
Find the `const PL=[` array and add:

```javascript
{
  id:'p13',          // unique id
  n:'Plant name',
  s:'Scientific name',
  ly:'Shrub 1–2m',  // layer description
  lc:'pt',          // layer pill colour: pt=terra (shrub), pg=green (tree)
  bl:'Year-round',  // blooming / fruiting period
  bc:'pg',          // blooming pill colour: pg=green (year-round), pa=amber (seasonal)
  pr:'high',        // 'keystone' or 'high'
  w:'Low–moderate', // water demand
  ph:'6.0–7.5',     // soil pH range
  sp:'1.5–2 m',     // spacing
  su:'Full sun',    // sunlight requirement
  bi:['Bird 1','Bird 2'],  // birds attracted
  no:'Care notes specific to Bengaluru / South India conditions.'
}
```

### Changing the default location
Find these two lines in `index.html` and update both:

```html
<!-- In the header chip -->
<span>Bengaluru, KA</span>

<!-- In the scan panel location box -->
<div class="lname">Bengaluru, Karnataka — 12.97°N, 77.59°E</div>
<div class="lcoord">Auto-detected · 10 km scan radius active</div>
```

### Changing the colour theme
All colours are CSS variables at the top of the `<style>` block. Change any of these:

```css
:root {
  --g:#2d5a3d;    /* Primary green — header, active nav, buttons */
  --gdk:#1e3f2b;  /* Dark green — button hover */
  --glt:#e8f2ec;  /* Light green — highlighted backgrounds */
  --tr:#c4663a;   /* Terra / warm orange — shrub layer pills */
  --am:#c9860d;   /* Amber — seasonal / warning indicators */
  --bl:#2e6fa3;   /* Blue — water / information */
  --pu:#6b4fa0;   /* Purple — nectarivore accent colour */
}
```

---

## 🗺 V1 → V3 Roadmap

### V1 (current)
- [x] 7 fully interactive tabs with inline drawers throughout
- [x] 9 nectarivore species with complete profiles
- [x] 12 keystone plants with full care specifications
- [x] Soil amendment calculator for Bengaluru red laterite
- [x] Seasonal irrigation demand curves
- [x] 12-month maintenance calendar with full task checklists
- [x] 7 measurable ecological impact metrics with mechanism explanations
- [x] Cross-linking between species profiles and plant details
- [x] Toast notification system throughout
- [x] GitHub Pages auto-deployment

### V2 (planned — 3–6 months)
- [ ] Real GPS detection via browser Geolocation API
- [ ] Live eBird API integration — location-based species pull by coordinates
- [ ] iNaturalist API integration — adds butterflies, reptiles, pollinators
- [ ] All 7 ecological guilds with full species profiles (currently 2 complete)
- [ ] PDF export — Biodiversity Impact Report as downloadable document
- [ ] Excel / CSV planting list download
- [ ] Project save / load via browser localStorage

### V3 (platform — 12–18 months)
- [ ] User accounts and authentication
- [ ] Multi-project dashboard
- [ ] Pan-India regional data (current scope: Bengaluru / South India)
- [ ] Biodiversity score benchmarking across your own project portfolio
- [ ] Client-facing shareable report links (unique URL per project)
- [ ] GIS / map layer export

---

## 🌱 Ecological data sources

| Source | Data type | API docs |
|--------|-----------|----------|
| eBird (Cornell Lab) | Bird species occurrence by location | https://documenter.getpostman.com/view/664302/S1ENwy59 |
| iNaturalist | Multi-taxa biodiversity observations | https://api.inaturalist.org/v1/docs/ |
| GBIF | Global species occurrence records | https://www.gbif.org/developer/summary |
| India Biodiversity Portal | Native species validation for India | https://indiabiodiversity.org |
| Botanical Survey of India | Flora documentation | https://bsi.gov.in |

---

## ⚠️ Known limitations in V1

- **Location is hardcoded** — GPS auto-detection and live API queries are not yet implemented. Location shows Bengaluru as default. Real API integration is the primary V2 feature.
- **Species data is curated** — the 9 nectarivores are fully profiled. The other 6 guilds show species names and design guidance but do not yet have full profile drawers (V2).
- **Plant list covers nectarivore attractants** — the full 42-plant list referenced in metrics will be expanded in V2. Current list covers the 12 keystone nectar and canopy species.
- **Data is South India / Deccan Plateau specific** — soil profiles, plant recommendations, and species lists are calibrated for Bengaluru and surrounding region. Pan-India expansion is V3.

---

## 📄 Licence

© Greenscape Designz. All rights reserved.

The ecological intelligence, species-habitat rules engine, planting frameworks, and design methodology in this codebase are proprietary intellectual property developed by Greenscape Designz.

The frontend code structure (HTML/CSS/JS) may be adapted for non-commercial educational purposes with attribution to Greenscape Designz.

Commercial use, resale, or derivative products require written permission.

---

## 🤝 Collaboration

This is a studio-internal tool in active development. Landscape architects interested in contributing species data for other Indian regions (Mumbai, Chennai, Hyderabad, Delhi) — contact via [greenscapedesignz.com](https://greenscapedesignz.com).

---

*VANA — turning ecological intelligence into landscape design.*
