# Ironhaven Safety Intelligence

**Real-Time Underground Tunnel Safety Monitoring Dashboard**

> Inspired by the Teesta Stage-VI tunnel disaster (July 2026, Sikkim).

![Status](https://img.shields.io/badge/status-live-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![Languages](https://img.shields.io/badge/languages-23-orange)

## What It Does

Ironhaven is a **single-file safety monitoring dashboard** for underground tunnel construction. It provides continuous environmental monitoring, bi-directional worker–admin communication, and automated danger alerts — all in one HTML file that runs in any browser with **zero dependencies**.

### Key Features

- **9 Live Sensors** — Temperature, CO, Methane, Smoke, Humidity, AQI, Vibration, CO₂, Light Intensity with real-time sparkline charts
- **5 Tunnel Zones** — Intake Shaft, Main Bore, Deep Excavation, Tunnel Face, Ventilation Bay
- **12 Simulated Workers** — With roles, zone tracking, and safety gear checklists
- **23 Languages** — All 22 Scheduled Languages of India + Tibetan (Bhotia)
- **Hazard Reporting** — Workers report 9 types of hazards with voice/photo attachments
- **Two-Way Communication** — Admin receives reports, sends instructions back to workers
- **Audio Alerts** — Web Audio API sirens (800Hz warning beep, 600Hz+900Hz danger siren)
- **Emergency Contacts** — 5 defaults + custom contacts, persisted in localStorage
- **Dark/Light Theme** — System-aware with manual toggle

## Quick Start

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/ironhaven-safety-intelligence.git

# Open in browser — no server needed
open public/index.html
```

Or visit the live deployment: **[ironhaven-safety-intelligence.vercel.app](https://ironhaven-safety-intelligence.vercel.app)**

## Architecture

```
ironhaven-safety-intelligence/
├── public/
│   └── index.html          # Complete dashboard (3000+ lines, self-contained)
├── data/
│   └── tunnel_monitoring_dataset_5000.csv
├── notebooks/
│   └── Ironhaven_Safety_Intelligence_ML.ipynb
├── docs/
│   └── ...
├── vercel.json              # Vercel deployment config
└── README.md
```

### How It Works

```
Sensors (9 params) → Gaussian Simulation → Threshold Detection → Alerts (Visual + Audio)
                                                                      ↓
Workers (12 crew) → Hazard Reports (voice/photo) → Admin Inbox → Instructions → Workers
```

## Sensor Engine

Each sensor uses a **Gaussian distribution model** with configurable danger thresholds:

| Sensor | Unit | Mean ± StdDev | Danger Threshold |
|--------|------|---------------|------------------|
| Temperature | °C | 30.04 ± 5.02 | 38°C |
| Carbon Monoxide | ppm | 19.96 ± 14.66 | 35 ppm |
| Methane | %LEL | 2.91 ± 2.13 | 5% LEL |
| Smoke Level | μg/m³ | 36.05 ± 27.19 | 70 μg/m³ |
| Humidity | %RH | 64.89 ± 9.89 | 85% RH |
| Air Quality Index | — | 102.84 ± 74.45 | 200 |
| Vibration | g | 0.05 ± 0.03 | 0.1 g |
| CO₂ Level | ppm | 509.59 ± 279.89 | 1000 ppm |
| Light Intensity | lux | 251.44 ± 79.2 | < 100 lux |

## Tech Stack

- **Frontend**: HTML5, CSS3 (Custom Properties, Flexbox, Grid), Vanilla ES6+ JavaScript
- **Charting**: Custom SVG sparkline renderer — zero library dependency
- **Audio**: Web Audio API (OscillatorNode + GainNode)
- **Media**: MediaRecorder API (voice), FileReader API (photos)
- **Persistence**: localStorage (thresholds, contacts, language, theme)
- **Deployment**: Vercel (static hosting, global CDN, auto-SSL)
- **Versions**: 23 published iterations

## Deployment

### Vercel (Recommended)

1. Push to GitHub
2. Import the repo on [vercel.com](https://vercel.com)
3. It auto-detects the `vercel.json` config — no build step needed
4. Your dashboard is live at `your-project.vercel.app`

### Manual

Just open `public/index.html` in any browser. No server, no build, no install.

## License

MIT — See [LICENSE](LICENSE) for details.

---

**Created by [Anamika Tripathi](mailto:anamikatripathi2023@gmail.com)**
