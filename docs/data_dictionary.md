# Data Dictionary — FIFA World Cup Players Dataset
This data dictionary documents the cleaned dataset used for analysis and Tableau dashboarding in the FIFA World Cup Analytics project.

## Dataset Summary

| Item | Details |
|---|---|
| Dataset name | FIFA World Cup Players (Cleaned) |
| Source | Kaggle — FIFA World Cup Dataset |
| Raw file name | WorldCupPlayers.csv |
| Last updated | 21 April 2026 |
| Granularity | One row per player per match |

---

## Column Definitions

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|---|---|---|---|---|---|
| RoundID | int | Unique identifier for tournament round | 201 | EDA / Tableau | Ensured numeric type |
| MatchID | int | Unique identifier for each match | 1096 | EDA / Tableau | Used for joins with match data |
| Team Initials | string | Country/team abbreviation | BRA | EDA / Tableau | Standardized text format |
| Coach Name | string | Name of the team’s coach | Didier Deschamps | EDA | Trimmed whitespace |
| Line-up | string | Player role in match (Starter/Substitute) | Starter | KPI / Tableau | Standardized categories |
| Shirt Number | int | Jersey number of player | 10 | EDA | Missing values retained |
| Player Name | string | Full name of player | Lionel Messi | EDA / Tableau | Cleaned formatting |
| Position | string | Player’s field position (GK, DF, MF, FW) | FW | KPI / Tableau | Missing values present |
| Event | string | Match events (goals, cards, etc.) | G40', Y45' | KPI | Null values imply no event |

---

## Derived Columns

| Derived Column | Logic | Business Meaning |
|---|---|---|
| Goal Indicator | Extract rows where Event contains 'G' | Identifies goal-scoring players |
| Card Indicator | Extract rows where Event contains 'Y' or 'R' | Tracks disciplinary actions |
| Player Participation Count | Count of MatchID per Player Name | Measures player involvement |

---

## Data Quality Notes

- Missing values present in **Position** and **Shirt Number**
- Event column is sparse (only filled for key events)
- Duplicate records were removed during cleaning
- Same player appears multiple times across matches (expected behavior)
- Some inconsistencies in text formatting were standardized