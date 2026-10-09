# Ironhaven Safety Intelligence

**Real-Time Underground Tunnel Safety Monitoring System**

> Inspired by the Teesta Stage-VI tunnel disaster (July 2026, Sikkim) — where 30+ workers were trapped underground with zero digital monitoring in place.

![Status](https://img.shields.io/badge/status-live-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![Languages](https://img.shields.io/badge/languages-23-orange) ![Version](https://img.shields.io/badge/version-v23-red)

**Live Demo:** [ironhaven-safety-intelligence.vercel.app](https://ironhaven-safety-intelligence.vercel.app)

---

## The Problem

India has over 200 active tunnel construction projects — highways, railways, hydroelectric dams — but not a single one uses a real-time digital safety monitoring system. When the Teesta Stage-VI tunnel collapsed in Sikkim in July 2026, rescue teams had:

- No sensor data from inside the tunnel
- No way to communicate with trapped workers
- No idea which zones were safe and which were dangerous

Workers in these tunnels come from all over India and speak 20+ different languages. Safety manuals written in English are useless to a Santali-speaking miner from Jharkhand or a Meitei worker from Manipur.

**Ironhaven was built to solve exactly this.**

---

## What Is Ironhaven?

Ironhaven is a complete safety monitoring dashboard packed into a **single HTML file**. You open it in any browser — on a phone, tablet, or computer — and it immediately starts working. No installation, no server, no internet dependency, no app store.

It does three things:

1. **Monitors the tunnel environment** — 9 different sensors (temperature, gas levels, smoke, vibration, etc.) across 5 tunnel zones, updating every 3 seconds
2. **Connects workers and administrators** — workers can report hazards (with voice recordings and photos), and admins can send safety instructions back — all in the worker's own language
3. **Alerts everyone when something goes wrong** — visual danger overlays and audio sirens that trigger automatically when sensor readings cross safe limits

---

## Key Features

### For Administrators
- **Live sensor dashboard** — a grid showing all 9 sensors across all 5 zones, color-coded by safety status (green = safe, amber = warning, red = danger)
- **Configurable thresholds** — adjust when warnings and danger alerts trigger for each sensor
- **Worker zone map** — see which workers are in which zone, with their roles and safety gear status
- **Hazard report inbox** — incoming worker reports sorted by urgency, with voice and photo attachments
- **Emergency contacts panel** — 5 default contacts + ability to add custom ones

### For Workers
- **23 languages** — the entire interface (every button, label, alert, and instruction) translates into the worker's native language, rendered in their native script
- **Hazard reporting** — report any of 9 hazard types with optional voice recording and photo evidence
- **Safety gear checklist** — zone-specific list of required PPE (hard hat, respirator, goggles, etc.)
- **Admin messages** — receive safety instructions from administrators in your own language
- **Danger overlay** — a full-screen red alert when your zone becomes unsafe

### Alert System
The alert system has three levels:
- **SAFE** (green) — all sensor readings are within normal range
- **WARNING** (amber) — a sensor has hit 80% of its danger threshold
- **DANGER** (red) — a sensor has crossed 100% of its threshold, triggering a visual overlay and an audio siren (dual-tone at 600Hz + 900Hz using the Web Audio API)

### Supported Languages
All 22 Scheduled Languages of India + Tibetan (Bhotia):

Hindi, Bengali, Telugu, Marathi, Tamil, Urdu, Gujarati, Kannada, Malayalam, Odia, Punjabi, Assamese, Maithili, Santali, Kashmiri, Nepali, Sindhi, Konkani, Dogri, Manipuri (Meitei), Bodo, Tibetan (Bhotia)

Every string in the UI — buttons, labels, alerts, instructions, safety checklists — is translated and renders in the correct script (Devanagari, Bengali, Telugu, Tamil, Gurmukhi, Tibetan, etc.).

---

## How It Works (Technical)

### Sensor Simulation
Since this is a prototype without real IoT hardware, the sensors are simulated using **Gaussian distribution models** (Box-Muller transform). Each sensor has a calibrated mean, standard deviation, and danger threshold based on real tunnel safety data from DGMS (Directorate General of Mines Safety) standards.

| Sensor | Unit | Normal Range (Mean ± StdDev) | Danger Threshold |
|--------|------|------------------------------|------------------|
| Temperature | °C | 30.04 ± 5.02 | 38°C |
| Carbon Monoxide | ppm | 19.96 ± 14.66 | 35 ppm |
| Methane | %LEL | 2.91 ± 2.13 | 5% LEL |
| Smoke Level | μg/m³ | 36.05 ± 27.19 | 70 μg/m³ |
| Humidity | %RH | 64.89 ± 9.89 | 85% RH |
| Air Quality Index | — | 102.84 ± 74.45 | 200 |
| Vibration | g | 0.05 ± 0.03 | 0.1 g |
| CO₂ Level | ppm | 509.59 ± 279.89 | 1000 ppm |
| Light Intensity | lux | 251.44 ± 79.2 | < 100 lux |

### Tunnel Zones
The tunnel is divided into 5 zones, each with different hazard profiles:

| Zone | Name | What Happens Here | Main Risk |
|------|------|-------------------|-----------|
| A | Intake Shaft | Entry point, fresh air supply | Poor ventilation |
| B | Main Bore | Primary tunnel passage | Gas accumulation |
| C | Deep Excavation | Deepest active digging area | Structural collapse |
| D | Tunnel Face | Where blasting/drilling happens | Blast overpressure |
| E | Ventilation Bay | Air circulation equipment | Heat exhaustion |

### Data Flow
```
Sensors (9 types × 5 zones)
    ↓ every 3 seconds
Gaussian Simulation (Box-Muller transform)
    ↓
Threshold Engine (80% = warning, 100% = danger)
    ↓
Alerts (visual overlay + audio siren)
    ↓
Worker Notifications (in their language)

Workers → Hazard Report (9 types + voice + photo)
    ↓
Admin Inbox (sorted by urgency)
    ↓
Admin Response (in worker's language)
    ↓
Worker Acknowledgement → Resolved
```

---

## Tech Stack

The entire application uses **zero external libraries**. Everything is built with native browser APIs:

| What | How | Why Native |
|------|-----|------------|
| UI Layout | HTML5 + CSS3 (Grid, Flexbox, Custom Properties) | No build step, works everywhere |
| Charts | Custom SVG sparkline renderer | No Chart.js or D3 dependency |
| Audio Sirens | Web Audio API (OscillatorNode + GainNode) | No audio files to load |
| Voice Recording | MediaRecorder API | No external recording library |
| Photo Capture | FileReader API | Native file handling |
| Data Persistence | localStorage | Settings survive browser close |
| Sensor Noise | Box-Muller transform (Gaussian random) | Realistic sensor behavior |
| Theming | CSS Custom Properties + data-theme attribute | Dark/light mode in ~10 lines |
| Deployment | Vercel static hosting | Global CDN, automatic SSL |

**By the numbers:**
- 3,311 lines of code (HTML + CSS + JS, single file)
- 23 versions published
- 9 live sensors
- 23 languages
- 12 simulated workers
- 0 external libraries

---

## Project Structure

```
ironhaven-safety-intelligence/
├── public/
│   └── index.html                              # The complete dashboard (3,311 lines)
├── data/
│   └── tunnel_monitoring_dataset_5000.csv       # 5,000-row synthetic dataset for ML training
├── notebooks/
│   └── Ironhaven_Safety_Intelligence_ML.ipynb   # ML classification & anomaly detection
├── src/                                         # Source components (modular reference)
│   ├── components/
│   ├── data/
│   └── utils/
├── Ironhaven_Pitch_Deck_v2.pdf                  # 10-page project presentation
├── vercel.json                                  # Vercel deployment config
└── README.md
```

---

## Getting Started

### Option 1: Just Open It
```bash
git clone https://github.com/anamikatripathi2023-ui/IronHeaven.git
cd IronHeaven
```
Then open `public/index.html` in any browser. That's it. No npm install, no build step, no server.

### Option 2: Deploy on Vercel
1. Fork this repo
2. Go to [vercel.com](https://vercel.com) and import it
3. Vercel auto-detects the config — click Deploy
4. Your dashboard is live at `your-project.vercel.app`

### Option 3: Visit the Live Demo
Just go to [ironhaven-safety-intelligence.vercel.app](https://ironhaven-safety-intelligence.vercel.app)

---

## Current Limitations (and What Comes Next)

This is a working prototype. Here's what it does and doesn't do yet:

| What's Missing | Why | What's Planned |
|----------------|-----|----------------|
| Real sensor hardware | This is a simulation, not connected to IoT devices | Integration with MQTT broker + industrial sensors (Honeywell, Dräger) |
| Multi-device sync | Data lives in one browser tab | Backend (Node.js or FastAPI) + WebSocket for real-time sync |
| User authentication | No login system — anyone can access admin view | Role-based access control (Admin / Supervisor / Worker) |
| GPS tracking | Workers are assigned to zones, not tracked by location | UWB indoor positioning or BLE beacons |

### Roadmap

**Phase 1 (3–6 months):** Real IoT sensor integration via MQTT, multi-device sync with WebSockets, authentication with role-based access, PWA support for offline-first usage

**Phase 2 (6–12 months):** ML-based anomaly detection (LSTM models), predictive alerts before thresholds are breached, computer vision for PPE compliance checking, NLP-based hazard classification

**Phase 3 (12–24 months):** Multi-tunnel command center, BIM 3D tunnel visualization, automated regulatory compliance reporting, smart helmet integration

---

## ML Component

The `notebooks/` folder contains a Jupyter notebook that trains classification models on the synthetic sensor dataset (5,000 rows). It covers:
- Data preprocessing and feature engineering
- Multi-class classification (Safe / Warning / Danger)
- Anomaly detection for unusual sensor patterns
- Model evaluation and threshold optimization

The dataset in `data/tunnel_monitoring_dataset_5000.csv` mirrors the sensor distributions used in the live dashboard.

---

## License

MIT — free to use, modify, and distribute. See [LICENSE](LICENSE) for details.

---

**Created by [Anamika Tripathi](mailto:anamikatripathi2023@gmail.com)**
B.Tech ECE, MMMUT Gorakhpur | 2026
