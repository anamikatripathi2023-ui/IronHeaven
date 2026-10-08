# Ironhaven Safety Intelligence

Real-time underground tunnel safety monitoring system with AI-powered calamity prediction.

## Overview

Ironhaven Safety Intelligence is a monitoring dashboard designed for underground tunnel operations. It collects sensor data across multiple zones, applies machine learning models to predict dangerous conditions (explosions, fires, structural failures), and provides real-time alerts to operators and workers.

## Architecture

```
├── public/              # Dashboard (self-contained HTML)
│   └── index.html       # Live monitoring dashboard
├── data/                # Sensor datasets
│   └── tunnel_monitoring_dataset_5000.csv
├── notebooks/           # ML analysis & model training
│   └── Ironhaven_Safety_Intelligence_ML.ipynb
└── docs/                # Documentation
```

## Dashboard Features

- **Tunnel Schematic** — SVG zone map with real-time status indicators (Normal/Warning/Danger)
- **9 Sensor Channels** — Temperature, CO, CO₂, Methane, Smoke, Humidity, AQI, Vibration, Light Intensity with sparkline charts
- **AI Prediction Engine** — Overall risk score with SHAP feature contribution bars
- **Explosion Risk Index** — Weighted composite (Temp×0.3 + Methane×0.4 + Vibration×0.3)
- **Calamity Threshold Table** — Decision rules from the XGBoost model with live breach detection
- **Alert Feed** — Chronological log of threshold exceedances and zone events
- **KPI Tiles** — Tunnel status, CO level, methane, active sensors, explosion risk, workers in zone
- **Dark/Light Theme** — Follows system preference with explicit toggle support

## ML Models

Three models trained and compared on the 5,000-reading dataset (Google Colab notebook):

| Model | Architecture | Key Config |
|-------|-------------|------------|
| **XGBoost** | Gradient boosted trees | 300 estimators, depth 8, lr 0.05 |
| **Random Forest** | Bagged decision trees | 300 estimators, depth 15, balanced weights |
| **LSTM** | Recurrent neural network | 128→64 units, sequence length 6, BatchNorm + Dropout |

### Feature Engineering (73+ features)

- Rolling 12-window statistics (mean, std, min, max)
- Rate-of-change per sensor
- Cross-sensor interactions: CO×Methane, CO×Smoke, Temp×Methane
- Lag features (1, 3, 6 steps)
- Composite risk scores:
  - `explosion_risk_score` = Temp×0.3 + Methane×0.4 + Vibration×0.3
  - `fire_risk_score` = CO×0.5 + Smoke×0.5

### Key Findings

- **Top danger predictors**: CO Level (r=0.54), Methane (r=0.40), Smoke Level (r=0.23)
- **Class distribution**: Normal 9.4%, Warning 49.7%, Danger 40.9%
- **SHAP explainability** identifies which sensor readings drive each danger prediction

## Dataset

5,000 readings at 5-minute intervals from 2026-04-01 to 2026-04-18:

| Sensor | Unit | Range | Mean ± Std |
|--------|------|-------|------------|
| Temperature | °C | 22–48 | 30.0 ± 5.0 |
| CO Level | ppm | 0–92 | 20.0 ± 14.7 |
| CO₂ Level | ppm | 0–1844 | 509.6 ± 279.9 |
| Methane | %LEL | 0–13 | 2.9 ± 2.1 |
| Smoke Level | μg/m³ | 0–178 | 36.1 ± 27.2 |
| Humidity | %RH | 27–96 | 64.9 ± 9.9 |
| AQI | — | 0–473 | 102.8 ± 74.5 |
| Vibration | g | 0–0.15 | 0.05 ± 0.03 |
| Light Intensity | lux | 9–520 | 251.4 ± 79.2 |

## Tech Stack

- **Dashboard**: Vanilla HTML/CSS/JS (self-contained, no build step)
- **ML Pipeline**: Python, scikit-learn, XGBoost, TensorFlow/Keras, SHAP
- **Fonts**: DM Sans + JetBrains Mono (Google Fonts)
- **Design**: Custom industrial dark theme with DGMS-compliant status colors

## Getting Started

1. Open `public/index.html` in a browser — no server needed
2. Upload `notebooks/Ironhaven_Safety_Intelligence_ML.ipynb` to Google Colab for model training
3. Upload `data/tunnel_monitoring_dataset_5000.csv` when prompted in the notebook

## License

MIT
