# ❄️ Freezer Drum Location Tracker

**Kinsale Site — Materials Management | DigiApps / C4E**

A self-contained, single-file web application for tracking drum locations across three freezers (71, 72, 73) at the Lilly Kinsale site. Deployed via GitHub Pages — no build pipeline, no server, no database required.

---

## The Problem

Drums stored in freezers are moved in SAP using the LT10 transaction but are all assigned the same generic storage bin number regardless of their physical position. When production levels are high, operators spend significant time inside freezers at cold temperatures searching for specific drums. This creates:

- **Health & safety risk** — extended operator exposure to freezer temperatures
- **Productivity loss** — time wasted locating drums before and during moves
- **No visibility** — no way to see at a glance how full a freezer is or where a specific drum sits

## The Solution

A visual floor plan of each freezer with a grid-based location system. Every drum gets a unique address (e.g., `F71-A-P07-S3`) mapped to its physical position. Operators can search, place, and remove drums with one click, and the map updates in real time.

---

## Features

### Core Tracker
- **Interactive floor plan** for each freezer showing 2 lanes × 11 pallet positions × 4 drum slots (88 drums per freezer, 264 total)
- **Door on the left** layout matching the real walk-in orientation (P01 at door, P11 at back wall)
- **FIFO placement logic** — system auto-assigns the next slot, filling from the back wall toward the door so the oldest drums are always nearest the exit
- **Search by SU** — enter a 7-digit Storage Unit number and the drum highlights instantly on the map with a gold glow
- **Place / Remove** drums with batch assignment and automatic location logging

### Batch Colour-Coding
- Each batch (format: `D` + 6 digits, e.g., `D796298`) is assigned a unique colour
- Drums on the map are colour-coded by batch for instant visual grouping
- **Batch filter bar** — click any batch chip to highlight only those drums; everything else dims
- Batch count shown as `x/5` since each batch typically contains 5 drums

### Capacity Alert System
Three-tier automated alerts when any freezer approaches capacity:

| Tier | Threshold | Action |
|------|-----------|--------|
| 🟡 WARNING | 85% | Monitor closely |
| 🟠 HIGH | 90% | Plan redistribution |
| 🔴 CRITICAL | 95% | Immediate action required |

- Visual alert banners appear on the Tracker tab
- Auto-opens a pre-filled email to `DigiApps@elililly.onmicrosoft.com` with full freezer status
- Subject line prefixed with `FreezerTracker:` for easy filtering
- Each tier triggers once per freezer; resets if capacity drops below the threshold

### Shift Snapshot (Printable)
- One-page printable summary of the current freezer state
- Includes: freezer name, timestamp, drum inventory by position, and batch breakdown
- Print-friendly layout strips navigation and controls automatically
- Designed to be carried into the freezer as a paper reference

### Utilisation Dashboard
- **Capacity gauges** — conic-gradient rings for Freezer 71, 72, 73, and combined
- **Batch distribution** — horizontal bar chart showing drum count per batch across all freezers
- **14-day trend chart** — simulated utilisation trend (canvas-rendered line chart with 3 series)
- **Lane heatmap** — colour intensity grid showing drum density across every position in all 3 freezers
- **All Freezers Available** — total available slots across the site, visible on both the Tracker and Dashboard tabs

### Movement Log
- Timestamped record of every placement and removal
- Columns: Time, Action (IN/OUT), SU Number, Batch, Freezer, Location
- Export to CSV for offline analysis or audit trail
- Colour-coded batch indicators in the log table

### How It Works Guide
- Built-in operator guide with all abbreviations defined (SU, LT10, FIFO, etc.)
- Analogies throughout to explain concepts in plain terms
- Step-by-step placement and removal instructions
- Capacity alert tier documentation

---

## Technical Details

| Attribute | Detail |
|-----------|--------|
| **Architecture** | Single-file HTML (React + Babel via CDN — no build pipeline) |
| **Deployment** | GitHub Pages (static hosting) |
| **Data Storage** | In-memory JavaScript (resets on page refresh) |
| **Dependencies** | IBM Plex Sans (Google Fonts CDN) |
| **Branding** | Eli Lilly corporate guidelines (Lilly Red `#D31710`, IBM Plex Sans, WCAG AA compliant) |
| **Responsive** | Mobile-friendly with adapted grid at 768px breakpoint |
| **Print Support** | CSS `@media print` rules for Shift Snapshot |
| **Browser Support** | Modern browsers (Chrome, Edge, Firefox, Safari) |

### File Structure

```
/
├── index.html          # The complete application (single file)
└── README.md           # This file
```

### Naming Convention

Every drum location follows a 4-part address:

```
F71 - A - P07 - S3
 │    │    │     └─ Slot 3 (1–4 on the pallet, 2×2 grid)
 │    │    └────── Position 07 (P01 = door, P11 = back wall)
 │    └─────────── Lane A or B
 └──────────────── Freezer 71, 72, or 73
```

### SAP Integration Note

The suggested SAP storage bin format for LT10 is `F71-A-P07` (Freezer-Lane-Position), replacing the current generic bin number. This links the SAP transfer order to the physical grid location tracked by this tool.

---

## Data Formats

| Field | Format | Example |
|-------|--------|---------|
| Storage Unit (SU) | 7 digits | `6086037` |
| Batch Number | `D` + 6 digits | `D796298` |
| Location Address | `F##-L-P##-S#` | `F71-A-P07-S3` |

---

## Deployment

1. Clone this repository
2. Ensure the HTML file is named `index.html`
3. Enable GitHub Pages in the repository settings (Source: main branch, root)
4. Access at `https://<username>.github.io/<repo-name>/`

No build step, no npm install, no server configuration required.

---

## Demo Data

The application loads with sample data across all 3 freezers:

- **Freezer 71** — Batches D796298 and D796315 (5 drums each)
- **Freezer 72** — Batch D796402 (5 drums)
- **Freezer 73** — Batch D796478 (5 drums)

This allows immediate exploration of all features without manual data entry.

---

## Future Enhancements

Potential additions identified during development:

- **FIFO "Next Out" indicator** — highlight the oldest drum for retrieval
- **Time-in-freezer tracking** with hold-time expiry alerts
- **Operator ID logging** for deviation investigations
- **Pallet detail view** — expandable 2×2 physical drum arrangement
- **SAP bin auto-generation** — copy-paste-ready bin strings for LT10
- **Persistent storage** — localStorage or SharePoint list integration for data that survives page refresh

---

## Support

For issues, feature requests, or questions:

**DigiApps Team** — [DigiApps@elililly.onmicrosoft.com](mailto:DigiApps@elililly.onmicrosoft.com?subject=FreezerTracker%3A%20)

---

*Freezer Drum Location Tracker v3.1 · Kinsale Site · Materials Management · DigiApps / C4E*
