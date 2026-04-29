# NST-DVA-CAP2 — FIFA World Cup Analytics

> A dark-themed, neon-accented interactive Tableau dashboard uncovering patterns in team dominance, player roles, attendance trends, and goal-scoring across 84 years of World Cup history (1930–2014).

![Phase](https://img.shields.io/badge/Phase-5%20Complete-brightgreen)
![Tool](https://img.shields.io/badge/Tool-Tableau%20Public-blue)
![Theme](https://img.shields.io/badge/Theme-Dark%20Neon-blueviolet)
![Domain](https://img.shields.io/badge/Domain-Sports%20Analytics-orange)
![Status](https://img.shields.io/badge/Status-Submitted-brightgreen)

---

## Project Overview

| Field | Details |
|---|---|
| **Project Name** | FIFA World Cup Analytics Dashboard |
| **Repo** | `NST-DVA-CAP2` |
| **Team** | Section D – Group 11 |
| **Institute** | Newton School of Technology |
| **Domain** | Sports Analytics |
| **Visualization Tool** | Tableau Public |
| **Dashboard Theme** | Dark / Neon |
| **Faculty Mentors** | Archit Raj & Satyaki Das |
| **Submission Date** | 29.04.2026 |
| **Status** | All phases complete — submitted |

---

## Team

| Name | Enrollment No. | Role |
|---|---|---|
| Shitanshu Singh | 2401020106 | Project Lead |
| Shivam Mishra | 2401010442 | Tableau |
| Palak Gupta | 2401010312 | Report & PPT |
| Aditya Samadhiya | 2401010037 | Data Cleaning |
| Krishna Gehlot | 2401010236 | Tableau |
| Prajjwal Tripathi | 2401010331 | ETL |

---

## Problem Statement

FIFA World Cup data spanning 84 years remains underutilized for visual storytelling. This project builds a dark-themed, neon-accented Tableau dashboard using 3 interconnected datasets — players, matches, and tournaments — to uncover patterns in team dominance, player roles, attendance trends, and goal-scoring across World Cup history.

---

## Datasets

All 3 datasets sourced and placed in `data/raw/`.

| Folder | File | Rows | Columns | Role |
|---|---|---|---|---|
| `data/raw/Primary/` | `WorldCupPlayers.csv` | 37,784 | 9 | Primary |
| `data/raw/Backup1/` | `WorldCupMatches.csv` | ~4,572 | 13 | Backup 1 |
| `data/raw/Backup2/` | `WorldCups.csv` | 20 | 10 | Backup 2 |

**Source:** [Kaggle — FIFA World Cup Dataset](https://www.kaggle.com/datasets/abecklas/fifa-world-cup)

---

## Repository Structure

```
NST-DVA-CAP2/
│
├── data/
│   ├── processed/              # cleaned_worldcup_players.csv, tableau_ready_worldcup_players.csv
│   └── raw/
│       ├── Primary/            # WorldCupPlayers.csv
│       ├── Backup1/            # WorldCupMatches.csv
│       └── Backup2/            # WorldCups.csv
│
├── docs/
│   └── data_dictionary.md      # Complete — all raw + engineered columns documented
│
├── DVA-focused-Portfolio/
│   └── README.md
│
├── DVA-oriented-Resume/
│   └── README.md
│
├── notebooks/
│   ├── 01_extraction.ipynb             # Phase 2 — Load raw CSVs
│   ├── 02_cleaning.ipynb               # Phase 2 — Clean & validate
│   ├── 03_eda.ipynb                    # Phase 2 — Explore trends
│   ├── 04_statistical_analysis.ipynb  # Phase 3 — Calculate KPIs & run tests
│   └── 05_final_load_prep.ipynb       # Phase 3 — Export to processed/
│
├── reports/
│   ├── FiFAPPT.pdf                     # Final presentation (submitted)
│   ├── SectionD_Group11_FiFa.pdf       # Final project report (submitted)
│   ├── presentation_outline.md         # Slide-by-slide outline
│   ├── project_report_template.md      # Full report in markdown
│   └── README.md
│
├── scripts/
│   ├── __init__.py
│   └── etl_pipeline.py
│
├── tableau/
│   ├── screenshots/
│   └── dashboard_links.md
│
├── .gitignore
├── README.md
└── requirements.txt
```

---

## Phase Tracker

| Phase | Description | Status |
|---|---|---|
| Phase 1 | Problem statement · Dataset sourcing · Repo setup | Complete |
| Phase 2 | Data extraction · Cleaning · EDA (`notebooks/01–03`) | Complete |
| Phase 3 | Statistical analysis · Tableau load prep (`notebooks/04–05`) | Complete |
| Phase 4 | Dashboard build in Tableau (dark neon theme + filters) | Complete |
| Phase 5 | Final report · Presentation · Tableau Public publish | Complete |

---

## Notebooks Pipeline

```
01_extraction → 02_cleaning → 03_eda → 04_statistical_analysis → 05_final_load_prep
     |               |            |               |                      |
  Load raw       Clean &       Explore        Calculate               Export
   CSVs          validate      trends           KPIs               to processed/
```

---

## Dashboard Design

| Property | Value |
|---|---|
| Canvas | `#0D0D0D` (near black) |
| Primary neon | `#00F5FF` (cyan) |
| Secondary accent | `#39FF14` (neon green) |
| Highlight | `#FF00FF` (magenta) |
| Text / labels | `#F5F5F5` |
| Screens | 3 dashboard sheets |
| Visuals | 7–10 charts across 3 screens |

**Dashboards:**

| # | Name | Focus |
|---|---|---|
| 1 | Team & Country Performance | Most successful nations, goals share, player appearances, yellow cards |
| 2 | Player Intelligence | Top scorers, most appeared, captains per nation, position distribution |
| 3 | Match Discipline & Substitutions | Starting XI vs subs, red cards, substitution patterns |

**Key Stats:**

| Metric | Value |
|---|---|
| Total Goals Analyzed | 1,056 |
| Unique Captains | 158 |
| Total Substitutions | 13,802 |
| Record Appearances | 33 (Miroslav Klose) |
| Most Goals — Nation | BRA — 210 |
| Foreign Coach % | 10.31% |

---

## Statistical Methods

| Test | Variables | Result |
|---|---|---|
| Mann-Whitney U | Goals: Starters vs Substitutes | Starters score significantly more — p < 0.05 |
| Chi-Square | Line-up vs yellow card likelihood | Starting players more likely to receive yellow cards |
| Pearson Correlation | Goals vs yellow cards (team level) | Strong positive correlation |
| Mann-Whitney U | Captains vs non-captains (discipline) | Captains receive more yellow cards |

---

## Links

| Resource | Link |
|---|---|
| Dataset source | [Kaggle — FIFA World Cup](https://www.kaggle.com/datasets/abecklas/fifa-world-cup) |
| Tableau Dashboard | [View on Tableau Public](https://public.tableau.com/app/profile/shivam.mishra3999/viz/dva-tableau/Dashboard3) |
| Data Dictionary | [docs/data_dictionary.md](https://github.com/Silence91169/SectionD_Group11_FiFa/blob/main/docs/data_dictionary.md) |
| GitHub Repository | [Silence91169/SectionD_Group11_FiFa](https://github.com/Silence91169/SectionD_Group11_FiFa) |