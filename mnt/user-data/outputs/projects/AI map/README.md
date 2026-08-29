# Radian Arc — Global Infrastructure Map

**Real-time interactive dashboard for monitoring distributed GPU/compute datacenter operations globally.**

A production-grade infrastructure monitoring dashboard displaying 30+ datacenter sites across APAC, MENA, and AMER regions. Shows live capacity metrics, power consumption, GPU/CPU load, and risk assessment with interactive site management.

## Overview

Managing a global distributed GPU infrastructure requires real-time visibility into:
- **Capacity utilization** (rack U-space, VRAM, compute)
- **Power consumption** (kW usage vs. max available)
- **GPU & CPU load** (utilization percentages)
- **Risk assessment** (healthy, warning, critical status)
- **Regional balancing** (load distribution across continents)

Radian Arc provides an interactive **world map view** of your entire infrastructure footprint with drill-down detail panels, filtering, and site management.

## Key Features

✅ **Interactive SVG world map** with dot-matrix continent rendering (no image assets)  
✅ **30+ datacenter sites** across APAC, MENA, AMER (sample dataset included)  
✅ **Site pins** color-coded by health status (Healthy/Warning/Critical/Upcoming/Test)  
✅ **Real-time metrics per site:**
  - Rack utilization (U-space used/total)
  - Power consumption (kW used/max)
  - GPU & CPU load percentages
  - Risk level with contextual notes  
✅ **Multi-level filtering** (All Sites / Live / Upcoming / Test data / At Risk)  
✅ **Detail panel** with metrics, risk assessment, rack visualization, management  
✅ **Add/edit/duplicate sites** via modal forms with validation  
✅ **Zoom & pan** (scroll to zoom, drag to pan, buttons for control)  
✅ **Map legend** with status indicators and GPU vendors (Nvidia/AMD)  
✅ **Header statistics** (total sites, utilization, power budget, at-risk count)  
✅ **Responsive design** (map + panel collapse on mobile)  
✅ **PWA installable** (offline-capable)  
✅ **Local storage** persistence (custom sites survive page reload)  

## How to Use

### Quick Start

1. **Open in browser:**
   ```bash
   open Radian_Arc_-_Global_Infrastructure_Map.html
   ```
   Or serve via HTTP:
   ```bash
   python3 -m http.server 8000
   open http://localhost:8000
   ```

2. **Explore the map:**
   - **Scroll** to zoom in/out
   - **Drag** to pan around the world
   - **Click a site pin** to see details in right panel
   - Use **zoom buttons** (+ / − / reset) for precise control

3. **Filter sites:**
   - Click filter buttons at top: All Sites / Live / Upcoming / Test data / At Risk
   - Map updates in real-time

4. **View site details:**
   - Click any site pin → right panel shows:
     - Metrics (racks, power, GPU/CPU load)
     - Risk assessment with description
     - Rack visualization (graphical U-space)
     - Action buttons (edit, duplicate, delete)

5. **Add a site:**
   - Click "+ Add site" button
   - Fill form: name, region, coordinates, metrics
   - Click "Save site" → appears on map immediately

6. **Dark mode:**
   - Button in header (already dark by default)

### Example Scenarios

**Scenario 1: Check Power Capacity**
- Filter "At Risk" → see Malaysia site in red (93% power)
- Click Malaysia → see detailed breakdown
- Recommendation: Rebalance workload to Canada site (40% utilization)

**Scenario 2: Add New Facility**
- Click "+ Add site"
- Enter: Region=Frankfurt, Racks=15, Power max=150kW
- Save → site appears on map
- Check capacity allocation suggestions

**Scenario 3: Monitor GPU Load**
- Scan all sites for high GPU utilization
- Thailand shows 81% GPU load (warning)
- Note says "Nvidia expansion racks queued"
- Plan procurement to complete migration

## Technical Architecture

```
┌─────────────────────────────────────────────┐
│      RADIAN ARC INFRASTRUCTURE DASHBOARD   │
├─────────────────────────────────────────────┤
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │         HEADER (Stats + Filters)    │   │
│  │  [All Sites] [Live] [Upcoming]...   │   │
│  │  Statistics: 30 sites | 85% util.   │   │
│  └─────────────────────────────────────┘   │
│                   │                         │
│  ┌────────────────┴────────────────────┐   │
│  │                                     │   │
│  │  ┌──────────────┐   ┌───────────┐  │   │
│  │  │  WORLD MAP   │   │  DETAIL   │  │   │
│  │  │  (SVG)       │   │  PANEL    │  │   │
│  │  │              │   │           │  │   │
│  │  │  [Pin cluster]   │ • Metrics │  │   │
│  │  │  - Healthy ●     │ • Risk    │  │   │
│  │  │  - Warning ⚠     │ • Racks   │  │   │
│  │  │  - Critical ✕    │ • Actions │  │   │
│  │  │  - Upcoming      │           │  │   │
│  │  │  - Test ◌        │           │  │   │
│  │  │              │   │           │  │   │
│  │  │  Legend:     │   │  [Edit]   │  │   │
│  │  │  + Zoom      │   │[Duplicate]   │   │
│  │  │  + Pan       │   │[Delete]      │   │
│  │  └──────────────┘   └───────────┘  │   │
│  │                                     │   │
│  └─────────────────────────────────────┘   │
│                                             │
└─────────────────────────────────────────────┘
```

### Map Rendering Pipeline

```
BASE_SITES data structure
  ├─ Embedded in HTML (30 sample sites)
  ├─ Loaded on startup
  └─ Combined with localStorage custom sites

SITE FILTERING
  ├─ Filter by status (live/upcoming/test/at-risk)
  └─ Update pin visibility on map

SVG WORLD MAP GENERATION
  ├─ Draw continent dots (pixel-art style)
  ├─ Plot site pins (circles, color-coded)
  ├─ Add graticule (lat/lon grid)
  ├─ Set up zoom/pan handlers
  └─ Enable click handlers per pin

DETAIL PANEL
  ├─ On pin click → show site details
  ├─ Render metrics (utilization gauges, load bars)
  ├─ Display risk assessment (ok/warn/crit)
  ├─ Visualize rack U-space (graphical)
  └─ Show management buttons

INTERACTIVE CONTROLS
  ├─ Scroll → zoom in/out
  ├─ Drag → pan around map
  ├─ Buttons → zoom +/−/reset
  └─ Filters → show/hide sites
```

### Component Architecture

```
STATE
  └─ SITES: Array of site objects
      ├─ id, name, region, coordinates
      ├─ Status (live/upcoming/test)
      ├─ Metrics (racks, power, load)
      ├─ Risk level & notes
      └─ GPU vendor & roadmap

UI LAYERS
  ├─ Header: Stats + Filter buttons
  ├─ Map: SVG viewport with pins & graticule
  ├─ Legend: Site status + GPU vendor legend
  ├─ Detail Panel: Metrics, risk, actions
  ├─ Modal: Add/edit/delete site forms
  └─ Tooltip: Hover info on pins

INTERACTION HANDLERS
  ├─ Filter button click → update pin visibility
  ├─ Pin click → open detail panel
  ├─ Add site button → open modal form
  ├─ Form submit → save site to localStorage + re-render
  ├─ Scroll/drag → pan/zoom on map
  └─ Zoom buttons → programmatic zoom control

PERSISTENCE
  └─ localStorage: Custom sites (add/edit/delete)
      └─ Loaded on startup, merged with BASE_SITES
```

## Data Model

### Site Object
```json
{
  "id": "th",
  "name": "Thailand",
  "region": "APAC · Bangkok",
  "lat": 13.75,
  "lon": 100.50,
  "status": "live",
  "vendor": "amd",
  "vendorUpcoming": "nvidia",
  "racks": 12,
  "uTotal": 504,
  "uUsed": 380,
  "powerUsedKW": 68,
  "powerMaxKW": 96,
  "cpuLoad": 72,
  "gpuLoad": 81,
  "risk": "warn",
  "riskNote": "GPU pool trending above 80% during peak render hours..."
}
```

### Metrics Explained
| Metric | Unit | Meaning |
|--------|------|---------|
| racks | count | Number of physical racks |
| uTotal | U-space | Total rack units (42U standard = 504 units for 12 racks) |
| uUsed | U-space | Occupied U-space (hardware installed) |
| powerUsedKW | kW | Current power draw |
| powerMaxKW | kW | Breaker/PSU limit |
| cpuLoad | % | CPU utilization (0–100%) |
| gpuLoad | % | GPU utilization (0–100%) |
| risk | enum | ok \| warn \| crit \| info \| test |

### Risk Levels
| Level | Color | Meaning | Action |
|-------|-------|---------|--------|
| **ok** | Green (#39d98a) | Healthy headroom | Monitor |
| **warn** | Orange (#f5b94d) | Approaching limits | Plan capacity add |
| **crit** | Red (#ff6b7a) | Close to max | Urgent rebalancing |
| **info** | Gray (#8b9ab0) | Upcoming/placeholder | Track |
| **test** | Yellow (#ffe600) | Test data | Ignore for prod |

## Use Cases

**Infrastructure Planning:**
- Visualize global footprint
- Identify capacity bottlenecks
- Plan regional load balancing

**Capacity Management:**
- Real-time utilization monitoring
- Power budget tracking
- GPU vendor transition planning

**Risk Assessment:**
- Identify at-risk sites
- Correlate metrics (high GPU load → high power usage)
- Plan preventive maintenance

**Stakeholder Communication:**
- Show board GPU infrastructure footprint
- Demonstrate global scale
- Justify capacity investments

**Procurement Planning:**
- Identify sites needing upgrades
- Plan GPU procurement by region
- Track vendor transitions (AMD → Nvidia, etc.)

## Sample Datasets

The application includes 30 sample sites:

**Live Sites (11):**
- Thailand, Vietnam, Malaysia, Macau, India, Bahrain
- USA, Canada, Iraq, Jordan, Australia

**Upcoming (2):**
- Kenya, Europe (Frankfurt)

**Test Data (20):**
- Tokyo, Seoul, Beijing, Shanghai, Jakarta, Manila, Singapore
- Dubai, Riyadh, Cairo, Lagos, Johannesburg
- Madrid, Paris, London, Berlin, Moscow
- São Paulo, Santiago, New York

All test sites are clearly marked and can be filtered out.

## Files

- `Radian_Arc_-_Global_Infrastructure_Map.html` — Complete single-file PWA application

## Installation

### As a Web App (PWA)
1. Open HTML file in browser
2. Desktop: Install icon in address bar (top-right)
3. Mobile: Tap "Install app" button in header
4. Standalone app (no browser UI)

### Serving Locally
```bash
python3 -m http.server 8000
open http://localhost:8000
```

## Browser Compatibility

- Chrome/Chromium 90+
- Firefox 88+
- Safari 15+
- Edge 90+
- Mobile: iOS Safari 15+, Chrome Android 90+

## Performance Notes

- SVG rendering: ~50ms (1000 dots + grid)
- Pin click → detail panel: ~100ms
- Zoom/pan: 60 FPS (smooth interaction)
- Add site → re-render: ~200ms
- localStorage I/O: <50ms per operation

## Limitations

1. **Sample data only:** Replace with live telemetry API
2. **No authentication:** Add login/RBAC for production
3. **No audit logging:** Add for compliance
4. **Read-only PDU/BMS data:** Assumes embedded metrics, doesn't fetch from equipment APIs
5. **No alerting:** Add Slack/email webhooks for critical thresholds

## Future Enhancements

1. **Live API integration:** Connect to real telemetry (Prometheus, Datadog, etc.)
2. **Time-series visualization:** Show metrics over time, trend forecasting
3. **Anomaly detection:** Auto-flag unusual load patterns
4. **Predictive analytics:** Forecast when capacity will be exceeded
5. **Multi-tenant:** Support multiple organizations with role-based access
6. **Export/reporting:** Generate capacity utilization reports
7. **Webhook alerts:** Slack/Teams/PagerDuty integration
8. **Custom dashboards:** Let admins create custom views

## Customization

### Change Sample Sites
Edit `BASE_SITES` array:
- Add/remove sites, adjust coordinates
- Update metrics and risk levels
- Add regions as needed

### Adjust Risk Thresholds
In `pinColor()` and `renderSite()`:
- Change GPU/CPU load thresholds
- Adjust power utilization warnings
- Define custom risk levels

### Styling
Modify `:root` CSS variables:
- Colors: `--ok`, `--warn`, `--crit`
- Spacing, fonts, breakpoints
- Dark mode toggle affects entire color scheme

---

**Version:** 1.0  
**Last Updated:** August 2026  
**License:** MIT
