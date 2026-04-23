# 📓 Notebooks Pipeline — Log

> This document logs every stage of the data pipeline for the FIFA World Cup Players analysis project.

**Dataset:** `data/raw/Primary/WorldCupPlayers.csv`  
**Source:** [Kaggle — FIFA World Cup Dataset](https://www.kaggle.com/datasets/abecklas/fifa-world-cup)  
**Coverage:** FIFA World Cup 1930 – 2014

---

## Pipeline Overview

```
01_extraction → 02_cleaning → 03_eda → 04_statistical_analysis → 05_final_load_prep
     ↓               ↓            ↓               ↓                      ↓
  Load raw       Clean &       Explore        Hypothesis             Compute KPIs
   CSV           transform     visually        testing              & export for
                                                                     Tableau
```

---

## Stage Details

| # | Stage | Notebook | Input | Output | Status |
|---|-------|----------|-------|--------|--------|
| 1 | **Data Extraction** | `01_extraction.ipynb` | `data/raw/Primary/WorldCupPlayers.csv` | — (inspection only) | ✅ Complete |
| 2 | **Cleaning** | `02_cleaning.ipynb` | `data/raw/Primary/WorldCupPlayers.csv` | `data/processed/cleaned_worldcup_players.csv` | ✅ Complete |
| 3 | **EDA** | `03_eda.ipynb` | `data/processed/cleaned_worldcup_players.csv` | — (visualizations only) | ✅ Complete |
| 4 | **Statistical Analysis** | `04_statistical_analysis.ipynb` | `data/processed/cleaned_worldcup_players.csv` | — (tests & insights) | ✅ Complete |
| 5 | **Tableau Load Prep** | `05_final_load_prep.ipynb` | `data/processed/cleaned_worldcup_players.csv` | `data/processed/tableau_ready_worldcup_players.csv` | ✅ Complete |

---

## Stage 1 — Data Extraction (`01_extraction.ipynb`)

**Purpose:** Load the raw CSV, inspect its structure, and confirm suitability.

**Key Actions:**
- Loaded `WorldCupPlayers.csv` → 37,784 rows × 9 columns
- Inspected `df.head()`, `df.tail()`, `df.info()`, `df.describe()`
- Built null analysis table — found Position (89% null), Event (76% null)
- Counted unique values per column — 82 teams, 836 matches, 7,663 players
- Compared rows from earliest (1930) and latest (2014) eras
- Found 736 exact duplicate rows (1.95%)
- Decoded Event column notation: G=Goal, Y=Yellow, R=Red, I/IH=Sub In, O/OH=Sub Out, P=Penalty, W=OwnGoal, MP=MissedPenalty, RSY=2nd Yellow Red

**Findings:**
| Metric | Value |
|--------|-------|
| Total rows | 37,784 |
| Total columns | 9 |
| Countries | 82 |
| Unique players | 7,663 |
| Unique matches | 836 |
| Duplicates found | 736 |

---

## Stage 2 — Cleaning (`02_cleaning.ipynb`)

**Purpose:** Clean raw data, apply transformations, and export a processed file.

**Key Actions:**

| Step | Action | Rationale |
|------|--------|-----------|
| 1 | Snake_case column names | `Team Initials` → `team_initials`, etc. |
| 2 | Strip whitespace on all string columns | Remove invisible padding |
| 3 | Drop 736 exact duplicate rows | Identical rows carry no new information |
| 4 | Replace `Shirt Number == 0` with NaN | 0 means "unknown" in pre-numbering era |
| 5 | Position: NaN→Outfield, C→Outfield, GK→Goalkeeper, GKC→GK-Captain | Captain status tracked separately via `is_captain` (int 0/1) |
| 6 | Map Line-up: S→Starting, N→Substitute | Human-readable labels |
| 7 | Parse Event column into 10 structured columns | `goals`, `penalties_scored`, `own_goals`, `yellow_cards`, `red_cards`, `second_yellow_reds`, `total_dismissals`, `missed_penalties`, `sub_in`, `sub_out` |
| 7a | Include IH / OH half-time substitution codes in sub_in / sub_out | ~600 half-time sub events were previously missed |
| 7b | Subtract `second_yellow_reds` from `yellow_cards` | Avoid double-counting the yellow that triggers RSY |
| 7c | Add `total_dismissals` = `red_cards` + `second_yellow_reds` | Unified dismissal metric |
| 7d | Cast `is_goalkeeper`, `is_captain`, `sub_in`, `sub_out` → int (0/1) | Consistent numeric export in CSV |
| 8 | Extract Coach Nationality from `Coach Name` | Regex extraction of 3-letter code from parentheses |

### Event Codes — Full Inventory

| Code | Parsed? | Column | Notes |
|------|---------|--------|-------|
| G | ✅ | `goals` | Goal scored |
| P | ✅ | `penalties_scored` | Penalty goal |
| W | ✅ | `own_goals` | Own goal |
| Y | ✅ | `yellow_cards` | Yellow card (adjusted for RSY) |
| R | ✅ | `red_cards` | Straight red card |
| RSY | ✅ | `second_yellow_reds` | Red via second yellow |
| MP | ✅ | `missed_penalties` | Missed penalty |
| I | ✅ | `sub_in` | Substituted in |
| IH | ✅ | `sub_in` | Substituted in at half-time |
| O | ✅ | `sub_out` | Substituted out |
| OH | ✅ | `sub_out` | Substituted out at half-time |
| ETO | ❌ | — | Extra-time only marker; contextual, not a player action |
| N | ❌ | — | Administrative notation; not a match event |

**Output:**
- File: `data/processed/cleaned_worldcup_players.csv`
- Rows: 37,048 (after dedup)
- Columns: 22 (9 original + 13 engineered)

---

## Stage 3 — Exploratory Data Analysis (`03_eda.ipynb`)

**Purpose:** Visual exploration of trends, distributions, and patterns.

**Visualizations Produced:**

| # | Chart | Type | Key Insight |
|---|-------|------|-------------|
| 1 | Top 15 Countries by Appearances | Horizontal bar | BRA, GER, ARG, ITA dominate |
| 2 | Goals per Player-Match | Count plot | Most scorers net exactly 1 goal |
| 3 | Starting XI vs Substitutes | Pie + Bar | Roughly 50/50 split |
| 4 | Yellow & Red Cards by Team | Grouped bar | High-appearance teams get more cards |
| 5 | Position Distribution | Bar | Outfield > Goalkeeper > GK-Captain |
| 6 | Top 10 Coaches by Matches | Horizontal bar | Top coaches managed 20+ WC matches |
| 7 | Coach Nationality (Top 10) | Bar | BRA, ARG, ITA produce most coaches |
| 8 | Goals vs Yellow Cards (Team) | Scatter (bubble) | Positive trend; some teams are outlier disciplinary |
| 9 | Players with Most Appearances | Horizontal bar | Top players appeared in 20+ WC matches |
| 10 | Substitution Patterns | Bar | Sub-in ≈ Sub-out; most rows have no sub event |

---

## Stage 4 — Statistical Analysis (`04_statistical_analysis.ipynb`)

**Purpose:** Hypothesis testing, correlation analysis, and comparative studies.

**Tests Performed:**

| # | Test | Method | Hypothesis |
|---|------|--------|------------|
| 1 | Lineup vs Yellow Card association | Chi-Square (χ²) | H₁: Starting players are more likely to get carded |
| 2 | Starters vs Substitutes — Goals | Mann-Whitney U (one-sided) | H₁: Starters score more goals on average |
| 3 | Team-level correlation matrix | Pearson / Spearman | Goals, yellow cards, red cards, matches played |
| 4 | Goals per Match ranking | Descriptive (no test) | Top 15 most efficient scoring teams (min 5 matches) |
| 5 | Captain Effect on Yellow Cards | Mann-Whitney U (two-sided) | H₁: Captains have different card rates |
| 6 | Foreign Coach — Goal Scoring | Mann-Whitney U (two-sided) | H₁: Foreign-coached teams score differently |

**Key Statistical Findings:**
- Starting players receive significantly more yellow cards than substitutes (expected — more minutes played)
- Starting players score significantly more goals than substitutes
- Strong positive correlation between total goals and total yellow cards at team level (confounded by matches played)
- Captains tend to receive more yellow cards than non-captains

---

## Stage 5 — Tableau Load Prep (`05_final_load_prep.ipynb`)

**Purpose:** Compute KPIs, add team-level metrics, and export Tableau-ready flat file.

**KPIs Added:**

| KPI Column | Logic |
|------------|-------|
| Team Total Goals | SUM(goals) per team |
| Team Total Yellow Cards | SUM(yellow_cards) per team |
| Team Total Red Cards | SUM(red_cards) per team |
| Team Total Dismissals | SUM(total_dismissals) per team |
| Team Matches Played | COUNT DISTINCT(matchid) per team |
| Team Unique Players | COUNT DISTINCT(player_name) per team |
| Team Goals/Match | team_total_goals / team_matches_played |
| Team Cards/Match | (team_yellows + team_reds) / team_matches_played |

**Boolean Flags for Tableau Filters (all int 0/1):**

| Flag | Logic |
|------|-------|
| Is Goalscorer | goals > 0 |
| Is Carded | yellow_cards + red_cards > 0 |
| Is Starter | line_up == "Starting" |
| Is Substitute Used | sub_in == 1 |
| Is Penalty Taker | penalties_scored > 0 OR missed_penalties > 0 |
| Foreign Coach | team_initials != coach_nationality |

**Output:**
- File: `data/processed/tableau_ready_worldcup_players.csv`
- Columns renamed to Title Case for Tableau display
- 40 columns total

---

## Data Quality Notes

1. **Position column** only records GK/Captain — no distinction between defenders, midfielders, forwards. Captain mapped to Outfield (captaincy tracked via `is_captain`)
2. **Shirt Number = 0** in early tournaments (pre-1950s) — converted to NaN
3. **736 exact duplicates** were removed during cleaning
4. **Event column** parsed via regex — complex multi-event strings like `G43' G87' Y90'` were correctly decomposed
5. **IH/OH half-time subs** included alongside regular I/O subs (~600 additional events captured)
6. **RSY yellow card adjustment** — subtracted `second_yellow_reds` from `yellow_cards` to avoid double-counting
7. **Coach nationality** extracted from `Coach Name` field format `SURNAME Name (NAT)`
8. **ETO and N codes** identified in event data but not parsed (rare contextual/administrative markers, not player actions)
9. Some country codes changed over time (e.g., `FRG`→`GER`, `URS`→`RUS`, `TCH`→`CZE`, `YUG`→`SCG`→`SRB`) — kept as-is to preserve historical accuracy

---

## Files Modified

| File | Action |
|------|--------|
| `notebooks/01_extraction.ipynb` | Populated with extraction logic |
| `notebooks/02_cleaning.ipynb` | Populated with cleaning pipeline |
| `notebooks/03_eda.ipynb` | Populated with 10 EDA visualizations |
| `notebooks/04_statistical_analysis.ipynb` | Populated with 6 statistical tests |
| `notebooks/05_final_load_prep.ipynb` | Populated with KPI computation & Tableau export |
| `notebooks/readme.md` | This file — pipeline log |
| `data/processed/cleaned_worldcup_players.csv` | Generated by notebook 02 |
| `data/processed/tableau_ready_worldcup_players.csv` | Generated by notebook 05 |
