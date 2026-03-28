# 🗺️ Delivery Zone Generator

**Auto-generate interactive delivery zone maps for any UK restaurant.** Enter your postcode, set your delivery radius, and get a print-ready zone map with smart sector pairing in seconds.

[![Live Demo](https://img.shields.io/badge/Live-Demo-brightgreen?style=for-the-badge)](https://ironbite2023.github.io/delivery-zone-generator/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-orange?style=for-the-badge)](#)

---

## ✨ Features

- **🏪 Universal** — Works for any UK restaurant. Enter name + postcode → get your map
- **📍 Real Data** — Uses [postcodes.io](https://postcodes.io) API for accurate GPS coordinates
- **🧮 Smart Clustering** — Angular clustering creates natural delivery corridors (pie slices from your shop)
- **↔️ Swing Sectors** — Identifies borderline postcodes that can go on adjacent runs
- **🖨️ Print-Ready** — Page 1: full-page map · Page 2: zone reference card for the wall
- **🔒 Zero Dependencies** — Single self-contained HTML file, no frameworks, no backend
- **⚡ Fast** — Generates in ~5 seconds using just 3 API calls

## 🎯 The Problem

Delivery restaurants waste time and fuel when drivers don't know which postcode sectors can share a run. Staff constantly ask: *"Can CV3 2 go with CV2 4?"*

This tool solves that by:
1. Auto-discovering every postcode sector within your delivery radius
2. Grouping them into logical delivery zones (N, NE, E, SE, S, SW, W)
3. Providing an interactive map where you click any two sectors to instantly check compatibility

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

## 📸 Screenshots

### Setup Page
Enter your restaurant name, postcode, and max delivery radius (3–10 miles).

### Generated Map
Voronoi tessellation with colour-coded angular zones, swing sector hatching, and an interactive click-to-pair system.

### Print Layout
Page 1: Full-page map for the dispatch area. Page 2: Zone reference card for the wall.

## 🔧 How It Works

```
User enters postcode (e.g. "CV1 5DW")
         │
         ▼ API Call 1
postcodes.io → Restaurant GPS + city + ward
         │
         ▼ API Call 2
Nearest outcodes → CV1, CV2, CV3, CV4, CV5, CV6...
         │
         ▼ API Call 3
Bulk reverse geocode → ~3000 postcodes discovered
         │
         ▼ Client-side
Group into sectors → Filter by radius → Angular cluster
         │
         ▼
Voronoi map + Zone cards + Click-to-pair logic
```

### Angular Clustering Algorithm

Unlike basic k-means (which creates random blobs), we use **angular clustering**:

1. Calculate the compass bearing from your shop to each sector
2. Divide the 360° compass into equal pie slices
3. Each slice = one delivery run direction
4. Sectors in the same direction always end up in the same zone

This creates **natural delivery corridors** — the driver heads in one direction and delivers everything along the way.

### Swing Sectors

Sectors on zone borders are automatically detected and marked with a `↔` symbol. These can go on either adjacent run, giving dispatchers flexibility.

## 🖨️ Printing

Click **Print / PDF** to get a two-page layout:
- **Page 1**: Full-bleed map (fills entire A4)
- **Page 2**: Zone reference card with large, wall-readable text

Laminate both pages and stick them on the dispatch wall. Drivers check the zone card instantly.

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Frontend | Vanilla HTML/CSS/JavaScript |
| Mapping | SVG with Voronoi tessellation |
| Data | [postcodes.io](https://postcodes.io) (free, no auth) |
| Clustering | Custom angular clustering algorithm |
| Fonts | [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts |

**Zero external dependencies.** No React, no Leaflet, no Mapbox. Just one HTML file.

## 📋 API Usage

This tool uses the free [postcodes.io](https://postcodes.io) API:
- `GET /postcodes/{postcode}` — Restaurant lookup
- `GET /outcodes/{outcode}/nearest` — Area discovery
- `POST /postcodes` (geolocations) — Bulk reverse geocoding

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
- [ ] Add custom zone colours
- [ ] Allow manual sector reassignment (drag between zones)
- [ ] Export zone data as JSON/CSV
- [ ] Support for non-UK postcodes
- [ ] Estimated drive times via routing API

---

Built with ❤️ for independent delivery restaurants everywhere.
