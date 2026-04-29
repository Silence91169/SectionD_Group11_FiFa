# FIFA World Cup Analytics Dashboard — Project Report

**Course:** Data Visualization & Analytics (DVA)
**Institute:** Newton School of Technology
**Submission Date:** 29.04.2026

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

**Faculty Mentors:** Mr. Archit Raj · Mr. Satyaki Das

**Project Resources:**
- GitHub: https://github.com/Silence91169/SectionD_Group11_FiFa
- Tableau Dashboard: https://public.tableau.com/app/profile/shivam.mishra3999/viz/dva-tableau/Dashboard3

---

## Executive Summary

This project analyzes historical FIFA World Cup player-level data (1930–2014) to uncover patterns in team composition, player roles, and match-level events that influence overall team performance and tournament outcomes.

The dataset was sourced from Kaggle. A complete ETL pipeline was implemented in Python using pandas, NumPy, and statistical methods. Cleaned data was visualized via 3 interactive Tableau dashboards covering player, match, and team-level insights.

**Key findings:** Starting lineup selection, goal-scoring concentration among key players, and disciplinary behavior are critical factors in team performance.

---

## Sector & Business Context

Sports analytics has evolved significantly — teams rely on data-driven insights to optimize player utilization and gain competitive advantage. Despite vast historical data availability, it is often underutilized for structured analysis and decision support. This project transforms raw World Cup data into actionable intelligence for coaches, analysts, and team management.

---

## Problem Statement & Objectives

**Problem:** Despite extensive FIFA World Cup historical data, teams rely on fragmented observations rather than comprehensive data-driven evaluation of player roles, participation patterns, and match events.

**Objectives:**
- Analyze player participation patterns across matches and tournaments
- Evaluate the impact of player roles (starter vs substitute) on match dynamics
- Identify trends in player positions and contribution to match events
- Uncover patterns in goals and disciplinary actions
- Support strategic insights for team composition and match preparation

**Scope:**
- In Scope: Player-level data analysis, role/position evaluation, trend identification, strategic insights, Tableau dashboards
- Out of Scope: Real-time prediction, assists/passes/fitness metrics, external factors (weather, injuries, finances)

---

## Data Description

**Source:** Kaggle — [FIFA World Cup Dataset](https://www.kaggle.com/datasets/abecklas/fifa-world-cup)
**Primary File:** `WorldCupPlayers.csv`
**Granularity:** One row per player per match

**Key Fields:**

| Column | Description |
|---|---|
| MatchID | Unique match identifier |
| RoundID | Tournament stage |
| Team Initials | Country/team code |
| Player Name | Player full name |
| Line-up | Starter (S) or Substitute (N) |
| Position | GK, DF, MF, FW |
| Event | Goals, cards, substitutions |

Full data dictionary: [`/docs/data_dictionary.md`](https://github.com/Silence91169/SectionD_Group11_FiFa/blob/main/docs/data_dictionary.md)

**Data Limitations:**
- No assists, passes, or player efficiency metrics
- Event data sparse — only major events captured (goals, cards)
- Missing values in Position (~89%) and Event (~76%) columns
- Historical data only — no real-time dynamics
- No external factors (injuries, weather, team strategy)

---

## Data Cleaning & ETL Pipeline

**Initial State (raw):** 37,784 rows · 9 columns · 736 duplicate records

### Steps Performed

**Column Standardization** — names converted to snake_case

**Duplicate Removal** — 736 rows (~1.95%) removed

**Whitespace & Formatting** — all string columns stripped and standardized

**Missing Value Handling:**
- Position → standardized to Outfield / Goalkeeper categories
- Event → null interpreted as no recorded match event
- Shirt Number 0 → converted to null (historical placeholder)

**Categorical Mapping:**
- Line-up: `S` → `Starting`, `N` → `Substitute`
- Position values normalized and enriched with flags

**Feature Engineering from Event column:**

| Extracted Feature | Description |
|---|---|
| goals, penalties_scored, own_goals | Offensive events |
| yellow_cards, red_cards, second_yellow_reds | Disciplinary events |
| sub_in, sub_out, missed_penalties | Substitution & penalty events |
| total_dismissals | red_cards + second_yellow_reds |
| is_goalkeeper, is_captain | Boolean flags |

Coach nationality extracted from Coach Name field via regex.

**Final Dataset:** 37,048 rows · 22 columns
Reference: [`notebooks/02_cleaning.ipynb`](https://github.com/Silence91169/SectionD_Group11_FiFa/blob/main/notebooks/02_cleaning.ipynb)

---

## KPI & Metric Framework

| KPI | Definition | Business Relevance |
|---|---|---|
| Team Total Goals | Total goals scored | Offensive performance |
| Team Total Yellow Cards | Total yellow cards | Disciplinary behavior |
| Team Total Red Cards | Total red cards | Critical infractions |
| Team Total Dismissals | Red + second yellow reds | Total player removals |
| Team Matches Played | Total matches | Context for normalization |
| Team Unique Players | Distinct players in squad | Squad rotation & utilization |
| Goals per Match | Total goals ÷ matches played | Scoring efficiency |
| Cards per Match | Total cards ÷ matches played | Discipline per game |

**Derived Boolean Flags:** Is Goalscorer · Is Carded · Is Starter · Is Substitute Used · Is Penalty Taker · Foreign Coach Indicator

---

## Exploratory Data Analysis (EDA)

**Team Participation & Dominance**
BRA, GER, ARG, ITA show consistently high participation — sustained squad depth over 84 years.

**Goal Scoring Patterns**
Most players score at most 1 goal per match. Scoring is concentrated among key attackers rather than distributed.

**Player Role Distribution**
Approximately balanced starters vs substitutes in volume, but starters contribute significantly more to key events (goals, cards) due to higher playtime.

**Disciplinary Trends**
Higher participation teams accumulate more yellow/red cards — partly match exposure, partly aggressive play style.

**Position Analysis**
Outfield players dominate over goalkeepers, reflecting natural football team composition.

**Correlation Insights**
Positive relationship observed between goals scored and yellow cards at team level — offensive teams play more aggressively.

Reference: [`notebooks/03_eda.ipynb`](https://github.com/Silence91169/SectionD_Group11_FiFa/blob/main/notebooks/03_eda.ipynb)

---

## Statistical Analysis

| Test | Variables | Finding |
|---|---|---|
| Chi-Square | Line-up vs yellow card likelihood | Starting players significantly more likely to receive yellow cards (p < 0.05) |
| Mann-Whitney U | Goals: Starters vs Substitutes | Starters score significantly more goals on average |
| Pearson/Spearman Correlation | Goals, cards, matches played | Strong positive correlation between goals and yellow cards (confounded by match count) |
| Mann-Whitney U | Captains vs non-captains (discipline) | Captains tend to receive more yellow cards |
| Comparative Tests | Foreign vs domestic coaching | Variation in goal-scoring patterns by coaching nationality |

**Summary:** Player roles (starter, substitute, captain) are statistically significant performance drivers across all three test types.

Reference: [`notebooks/04_statistical_analysis.ipynb`](https://github.com/Silence91169/SectionD_Group11_FiFa/blob/main/notebooks/04_statistical_analysis.ipynb)

---

## Tableau Dashboard Design

**Design Choices:** Dark theme for visual contrast · Bright accent colors for key metrics · Consistent layout across all screens · Chart types matched to data nature (distribution, comparison, ranking)

**Interactive Filters:** Team · Year (1930–2014 slider) · Position · Line-up (Starter/Substitute) · Foreign coach indicator

### Dashboard 1 — Tournament & Team Overview
- KPI Cards: Total goals, teams presented
- Bar Chart: Most Successful Nations
- Donut Chart: Goals Share — Top 6 Nations
- Area Chart: Player Appearances by Team
- Bar Chart: Yellow Cards by Team

### Dashboard 2 — Player-Level Analysis
- Treemap: Captains per Nation
- Pie Chart: Position Distribution (GK vs Outfield)
- Bar Chart: Top Scorers
- Bar Chart: Most Appeared Players

### Dashboard 3 — Substitution & Discipline Analysis
- KPI Cards: Substitutions, top substituting team, red cards, starters count
- Donut Chart: Starting XI vs Substitutes
- Bar Chart: Top Teams by Substitutions
- Bar Chart: Red Cards by Team
- Stacked Bar Chart: Yellow vs Red Cards ratio

**Live Dashboard:** https://public.tableau.com/app/profile/shivam.mishra3999/viz/dva-tableau/Dashboard3

---

## Insights Summary

1. BRA, GER, ARG, ITA show sustained dominance — strong squad depth across all tournaments
2. Goal scoring is concentrated among a small subset of key attacking players
3. Starting players contribute significantly more to goals and disciplinary events vs substitutes
4. Higher match participation correlates with higher card accumulation — aggressive play + exposure
5. Strong positive relationship between total goals and yellow cards at team level
6. Captains exhibit higher disciplinary involvement due to leadership role and match engagement
7. Substitutions are relatively infrequent — most match impact is driven by starting players
8. A small subset of players appear across large numbers of matches — high strategic importance
9. Certain countries consistently produce high-involvement coaches — structured management ecosystems
10. Coaching nationality influences goal-scoring patterns — strategy background affects outcomes

---

## Recommendations

1. **Optimize Starting Lineup Selection** — data-driven selection of starters directly improves match outcomes
2. **Focus on Key Goal-Scoring Players** — allocate resources around high-performing attackers
3. **Improve Disciplinary Control** — reduce unnecessary fouls, especially for high-intensity teams
4. **Leverage Experienced Players Strategically** — high match-participation players are critical in knockout stages
5. **Evaluate Coaching Strategy Diversity** — different tactical backgrounds improve adaptability
6. **Enhance Substitution Strategy** — reassess timing and effectiveness to maximize sub impact

---

## Impact Estimation

| Area | Estimated Impact |
|---|---|
| Scoring Efficiency | 5–10% improvement via optimized lineups and player targeting |
| Disciplinary Risk | Reduced dismissals → improved squad strength in critical matches |
| Strategic Utilization | Better in-game decisions via experienced player deployment |
| Match Preparation | Historical pattern analysis enables proactive tactical adjustments |

---

## Future Scope

1. **Advanced Performance Metrics** — assists, passes, distance covered, defensive actions
2. **Predictive Modeling** — ML models for match outcome and goal-scoring probability prediction
3. **Real-Time Data Integration** — live match dashboards for in-game decision support
4. **Enhanced Player Segmentation** — clustering to identify player archetypes and optimize composition
5. **Expanded Dataset Integration** — combine with match-level data, external factors (weather, injuries)
6. **Advanced Dashboarding** — drill-down capabilities, scenario-based analysis
7. **Tactical & Formation Analysis** — substitution timing, formation patterns, in-game strategy insights

---

## Conclusion

This project demonstrates how 84 years of FIFA World Cup data can be transformed into actionable insights through structured ETL, statistical validation, and interactive Tableau visualization. The analysis confirms that player roles, starting lineup selection, and disciplinary behavior are measurable drivers of team performance — and that data-driven approaches offer a genuine competitive edge in sports analytics.

---

## Appendix

**Repository Structure:**

| Path | Description |
|---|---|
| `notebooks/01_extraction.ipynb` | Data extraction |
| `notebooks/02_cleaning.ipynb` | Cleaning & ETL |
| `notebooks/03_eda.ipynb` | Exploratory Data Analysis |
| `notebooks/04_statistical_analysis.ipynb` | Statistical tests |
| `notebooks/05_final_load_prep.ipynb` | Final load preparation |
| `data/processed/cleaned_worldcup_players.csv` | Cleaned dataset |
| `scripts/etl_pipeline.py` | ETL pipeline script |
| `docs/data_dictionary.md` | Column-level data dictionary |

**Links:**
- GitHub: https://github.com/Silence91169/SectionD_Group11_FiFa
- Tableau: https://public.tableau.com/app/profile/shivam.mishra3999/viz/dva-tableau/Dashboard3