# 🗺️ Delivery Zone Generator

**Auto-generate interactive delivery zone maps and optimise delivery routes for any UK restaurant.** Enter your postcode, set your delivery radius, review your sectors, and get a print-ready zone map — or plan a delivery run and open it straight in Google Maps.

[![Live Demo](https://img.shields.io/badge/Live-Demo-brightgreen?style=for-the-badge)](https://ironbite2023.github.io/delivery-zone-generator/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-orange?style=for-the-badge)](#)

---

## ✨ Features

### 🗺️ Zone Map Mode
- **🏪 Universal** — Works for any UK restaurant. Enter name + postcode → get your map
- **📍 Real Data** — Uses [postcodes.io](https://postcodes.io) API for accurate GPS coordinates
- **📏 Custom Radius** — Set your max delivery radius (3–10 miles) to filter sectors
- **📋 Sector Review** — Review all discovered sectors with checkboxes before building. Uncheck any you don't want
- **🧮 Angular Clustering** — Sectors grouped into compass-based pie slices (N, NE, E, SE...) for natural delivery corridors
- **↔️ Swing Sectors** — Identifies borderline postcodes that can go on adjacent runs
- **🖱️ Click-to-Pair** — Click any two sectors on the map to instantly check if they can share a run
- **🖨️ Print-Ready** — Page 1: full-page map · Page 2: zone reference card for the wall

### 🚗 Route Planner Mode
- **🗂️ Dynamic Rows** — Clean, add/remove interface for up to 15 delivery stops
- **🏠 Exact Address Logic** — Enter "12 CV1 5DW"; the system uses the postcode for fast routing API logic, but keeps the house number for exact Google Maps navigation to the front door
- **🧠 Smart Ordering** — Nearest-neighbour algorithm optimises the stop sequence
- **🛣️ OSRM AI** — Integrates with the Open Source Routing Machine for real-world driving miles and minutes (not just straight-line distances)
- **💰 Fee Engine** — Set your custom `£/mile` and `Min Fee`. Automatically calculates delivery charges per stop and totals your round-trip revenue
- **📋 WhatsApp Export** — 1-click button to copy a perfectly formatted driver run-sheet (with return leg & driving durations) straight to your clipboard
- **📍 Google Maps** — One-tap button opens the full route in the Maps app with all stops pre-loaded

### Settings
- **💾 Auto-Save** — Your Restaurant Name, Postcode, Radius, and Fee Settings are saved to `localStorage` and persist across sessions automatically

### General
- **🔒 Zero Dependencies** — Single self-contained HTML file, no frameworks, no backend
- **⚡ Fast** — Zone map generates in ~10 seconds, route plans are instant

## 🎯 The Problem

Delivery restaurants waste time and fuel when drivers don't know which postcode sectors can share a run. Staff constantly ask: *"Can CV3 2 go with CV2 4?"* And when a run is assigned, drivers don't know the optimal order.

This tool solves both problems:

| Problem | Solution |
|---------|----------|
| "Which postcodes go together?" | **Zone Map** — visual compass-based delivery corridors |
| "What order should I deliver in?" | **Route Planner** — optimised sequence + Google Maps |

## 🚀 Quick Start

### Option 1: Use Online
Visit the [live demo](https://ironbite2023.github.io/delivery-zone-generator/) — no installation needed.

### Option 2: Run Locally
```bash
# Clone the repo
git clone https://github.com/ironbite2023/delivery-zone-generator.git

# Open in browser (it's just one HTML file)
open index.html
```

That's it. No `npm install`, no build step, no server required.

## 📸 How It Works

### Zone Map Flow

```
                    ┌──────────────────┐
                    │  Enter postcode  │
                    │  + radius (5mi)  │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  API discovers   │
                    │  all sectors     │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  📋 REVIEW       │
                    │  ☑ CV1 1  0.4mi  │
                    │  ☑ CV2 3  1.6mi  │
                    │  ☐ CV3 9  2.4mi  │ ← uncheck unwanted
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  Angular cluster │
                    │  into pie slices │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  Voronoi map +   │
                    │  zone cards +    │
                    │  click-to-pair   │
                    └──────────────────┘
```

### Route Planner Flow

```
                    ┌──────────────────┐
                    │  Enter restaurant│
                    │  + delivery PCs  │
                    │  CV2 4NZ         │
                    │  CV3 1HJ         │
                    │  CV6 5AB         │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  Batch lookup    │
                    │  via postcodes.io│
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  Nearest-neighbour│
                    │  route ordering  │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │  ★ CV1 5DW start │
                    │  1. CV2 4NZ      │
                    │  2. CV3 1HJ      │
                    │  3. CV6 5AB      │
                    │  ★ Return        │
                    │                  │
                    │  [Open Maps 📍]  │
                    └──────────────────┘
```

### Angular Clustering Algorithm

Unlike basic k-means (which creates random blobs), we use **angular clustering**:

1. Calculate the compass bearing from your shop to each sector
2. Divide the 360° compass into equal pie slices
3. Each slice = one delivery run direction
4. Sectors in the same direction always end up in the same zone

This creates **natural delivery corridors** — the driver heads in one direction and delivers everything along the way. Results are deterministic (same input = same output).

### Swing Sectors

Sectors on zone borders are automatically detected and marked with a `↔` symbol. These can go on either adjacent run, giving dispatchers flexibility.

## 🖨️ Printing

Click **Print / PDF** to get a two-page layout:
- **Page 1**: Full-bleed map (fills entire A4)
- **Page 2**: Zone reference card with large, wall-readable text

Laminate both pages and stick them on the dispatch wall.

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Frontend | Vanilla HTML/CSS/JavaScript |
| Storage | HTML5 `localStorage` (Zero Cookies) |
| Mapping | SVG with Voronoi tessellation |
| Geocoding | [postcodes.io](https://postcodes.io) (free, no auth) |
| Clustering | Custom angular clustering algorithm |
| Routing Logic| Nearest-neighbour TSP via postcodes.io |
| Navigation Engine | [OSRM (Open Source Routing Machine)](http://project-osrm.org/) API |
| Navigation Output | Google Maps multi-stop URLs |
| Fonts | [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts |

**Zero external dependencies.** No React, no Leaflet, no Mapbox. Just one HTML file.

## 📋 API Usage

This tool uses the free [postcodes.io](https://postcodes.io) API:
- `GET /postcodes/{postcode}` — Postcode lookup
- `GET /outcodes/{outcode}/nearest` — Area discovery
- `POST /postcodes` (geolocations) — Bulk reverse geocoding
- `POST /postcodes` (postcodes) — Batch postcode lookup

**No API key required.** postcodes.io is an open data service by Ideal Postcodes.

## 🗺️ Coverage

Works for any UK postcode. Tested with:
- Coventry (CV1–CV12)
- Birmingham (B1–B97)
- London (E, N, W, SW, SE, EC, WC)
- Manchester (M1–M90)

## 📄 License

MIT License — free for personal and commercial use.

## 🤝 Contributing

Contributions welcome! Ideas for improvement:
- [ ] Allow manual sector reassignment (drag between zones)
- [ ] Export zone data as JSON/CSV
- [ ] Support for non-UK postcodes (requires replacing `postcodes.io`)
- [ ] Driver assignment and split-routing for very large batches

---

Built with ❤️ for independent delivery restaurants everywhere.
