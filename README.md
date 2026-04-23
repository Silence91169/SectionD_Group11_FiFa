# ⚽ NST-DVA-CAP2 — FIFA World Cup Analytics

> A dark-themed, neon-accented interactive Tableau dashboard uncovering patterns in team dominance, player roles, attendance trends, and goal-scoring across 90+ years of World Cup history.

![Phase](https://img.shields.io/badge/Phase-4%20In%20Progress-yellow)
![Tool](https://img.shields.io/badge/Tool-Tableau%20Public-blue)
![Theme](https://img.shields.io/badge/Theme-Dark%20Neon-blueviolet)
![Domain](https://img.shields.io/badge/Domain-Sports%20Analytics-orange)
![Status](https://img.shields.io/badge/Status-Repo%20Scaffolded-lightgrey)

---

## 📌 Project Overview

| Field | Details |
|---|---|
| **Project Name** | FIFA World Cup Analytics Dashboard |
| **Repo** | `NST-DVA-CAP2` |
| **Domain** | Sports Analytics |
| **Visualization Tool** | Tableau Public |
| **Dashboard Theme** | Dark / Neon (Cyberpunk) |
| **Current Phase** | Phase 4 — Dashboard Build in Tableau |
| **Status** | ETL · EDA · Statistical Analysis complete · Tableau data ready |

---

## 🎯 Problem Statement

FIFA World Cup data spanning 90+ years remains underutilized for visual storytelling. This project builds a dark-themed, neon-accented Tableau dashboard using 3 interconnected datasets — players, matches, and tournaments — to uncover patterns in team dominance, player roles, attendance trends, and goal-scoring across World Cup history.

---

## 🗂️ Datasets

> ✅ All 3 datasets have been sourced and placed in `data/raw/`.

| Folder | File | Rows | Columns | Role | Status |
|---|---|---|---|---|---|
| `data/raw/Primary/` | `WorldCupPlayers.csv` | 37,784 | 9 | Primary | ✅ Added |
| `data/raw/Backup1/` | `WorldCupMatches.csv` | ~4,572 | 13 | Backup 1 | ✅ Added |
| `data/raw/Backup2/` | `WorldCups.csv` | 20 | 10 | Backup 2 | ✅ Added |

**Source:** [Kaggle — FIFA World Cup Dataset](https://www.kaggle.com/datasets/abecklas/fifa-world-cup)

---

## 📁 Repository Structure

```
NST-DVA-CAP2/
│
├── data/
│   ├── processed/              # ✅ cleaned_worldcup_players.csv, tableau_ready_worldcup_players.csv
│   └── raw/
│       ├── Primary/            # ✅ WorldCupPlayers.csv
│       ├── Backup1/            # ✅ WorldCupMatches.csv
│       └── Backup2/            # ✅ WorldCups.csv
│
├── docs/
│   └── data_dictionary.md      # ✅ Complete — all raw + engineered columns documented
│
├── DVA-focused-Portfolio/
│   └── README.md               # 🔜 To be filled after Phase 4
│
├── DVA-oriented-Resume/
│   └── README.md               # 🔜 To be filled after Phase 4
│
├── notebooks/
│   ├── 01_extraction.ipynb     # ✅ Complete — Phase 2
│   ├── 02_cleaning.ipynb       # ✅ Complete — Phase 2
│   ├── 03_eda.ipynb            # ✅ Complete — Phase 2
│   ├── 04_statistical_analysis.ipynb  # ✅ Complete — Phase 3
│   └── 05_final_load_prep.ipynb       # ✅ Complete — Phase 3
│
├── reports/
│   ├── presentation_outline.md # 🔜 To be filled in Phase 5
│   ├── project_report_template.md     # 🔜 To be filled in Phase 5
│   └── README.md               # 🔜 To be filled in Phase 5
│
├── scripts/
│   ├── __init__.py             # ✅ Complete
│   └── etl_pipeline.py         # ✅ Complete
│
├── tableau/
│   ├── screenshots/            # 🔜 Phase 4
│   └── dashboard_links.md      # 🔜 Phase 4
│
├── .gitignore                  # ✅ Created
├── README.md                   # ✅ You are here
└── requirements.txt            # ✅ Complete
```

---

## 🚦 Phase Tracker

| Phase | Description | Status |
|---|---|---|
| **Phase 1** | Problem statement · Dataset sourcing · Repo setup | ✅ Complete |
| **Phase 2** | Data extraction · Cleaning · EDA (`notebooks/01–03`) | ✅ Complete |
| **Phase 3** | Statistical analysis · Tableau load prep (`notebooks/04–05`) | ✅ Complete |
| **Phase 4** | Dashboard build in Tableau (dark neon theme + filters) | 🟡 In Progress |
| **Phase 5** | Final report · Presentation · Tableau Public publish | ⬜ Not Started |

---

## 📓 Planned Notebooks Pipeline

> All notebooks are complete. Phases 2 and 3 are done.

```
01_extraction → 02_cleaning → 03_eda → 04_statistical_analysis → 05_final_load_prep
     ↓               ↓            ↓               ↓                      ↓
  Load raw       Clean &       Explore        Calculate               Export
   CSVs          validate      trends           KPIs               to processed/
```

---

## 🎨 Planned Dashboard Design

| Property | Value |
|---|---|
| **Canvas** | `#0D0D0D` (near black) |
| **Primary neon** | `#00F5FF` (cyan) |
| **Secondary accent** | `#39FF14` (neon green) |
| **Highlight** | `#FF00FF` (magenta) |
| **Text / labels** | `#F5F5F5` |
| **Screens** | 3 dashboard sheets |
| **Visuals** | 7–10 charts across 3 screens |

---

## 🔗 Links

| Resource | Link |
|---|---|
| Dataset source | [Kaggle — FIFA World Cup](https://www.kaggle.com/datasets/abecklas/fifa-world-cup) |
| Tableau dashboard | *Next Phase*|
| Data dictionary | *NextPhase* |

---

## 👤 Author

