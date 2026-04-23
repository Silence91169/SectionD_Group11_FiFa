# Data Dictionary — FIFA World Cup Players Dataset
This data dictionary documents the cleaned dataset used for analysis and Tableau dashboarding in the FIFA World Cup Analytics project.

## Dataset Summary

| Item | Details |
|---|---|
| Dataset name | FIFA World Cup Players (Cleaned) |
| Source | Kaggle — FIFA World Cup Dataset |
| Raw file name | WorldCupPlayers.csv |
| Last updated | 23 April 2026 |
| Granularity | One row per player per match |

---

## Column Definitions

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|---|---|---|---|---|---|
| RoundID | int | Unique identifier for tournament round | 201 | EDA / Tableau | Ensured numeric type |
| MatchID | int | Unique identifier for each match | 1096 | EDA / Tableau | Used for joins with match data |
| Team Initials | string | Country/team abbreviation | BRA | EDA / Tableau | Standardized text format |
| Coach Name | string | Name of the team’s coach | Didier Deschamps | EDA | Trimmed whitespace |
| Line-up | string | Player role in match (Starter/Substitute) | S (Starting), N (Substitute) | KPI / Tableau | Standardized categories |
| Shirt Number | int | Jersey number of player | 10 | EDA | Missing values retained |
| Player Name | string | Full name of player | Lionel Messi | EDA / Tableau | Cleaned formatting |
| Position | string | Player’s field position (GK, DF, MF, FW) | Outfield, Goalkeeper, GK-Captain | KPI / Tableau | Missing values present |
| Event | string | Match events (goals, cards, etc.) | G40', Y45' | KPI | Null values imply no event |

---

## Derived Columns

| Derived Column | Logic | Business Meaning |
|---|---|---|
| Goal Indicator | Extract rows where Event contains 'G' | Identifies goal-scoring players |
| Card Indicator | Extract rows where Event contains 'Y' or 'R' | Tracks disciplinary actions |
| Player Participation Count | Count of MatchID per Player Name | Measures player involvement |

---

## 🔧 Engineered Columns — cleaned_worldcup_players.csv

These 13 columns were created during the cleaning pipeline (`02_cleaning.ipynb`) and are not present in the raw source file.

| Column Name | Data Type | Description | Example Value | Nullable |
|---|---|---|---|---|
| is_goalkeeper | int (0/1) | 1 if the player's position is GK or GKC (goalkeeper) | 1 | No |
| is_captain | int (0/1) | 1 if the player is captain (position C or GKC) | 0 | No |
| goals | int | Number of goals scored by this player in this match | 2 | No |
| penalties_scored | int | Number of penalty goals scored (P events) | 1 | No |
| own_goals | int | Number of own goals scored (W events) | 0 | No |
| yellow_cards | int | Yellow cards received; adjusted to subtract RSY to avoid double-counting | 1 | No |
| red_cards | int | Direct red cards received (R events only) | 0 | No |
| second_yellow_reds | int | Dismissals via second yellow card (RSY events) | 0 | No |
| total_dismissals | int | Combined dismissals: red_cards + second_yellow_reds | 0 | No |
| missed_penalties | int | Penalties missed (MP events) | 0 | No |
| sub_in | int (0/1) | 1 if the player was substituted into the match (I or IH events) | 1 | No |
| sub_out | int (0/1) | 1 if the player was substituted out of the match (O or OH events) | 0 | No |
| coach_nationality | object | 3-letter nationality code extracted from the Coach Name field via regex | BRA | Yes — null if code not parseable |

**Notes:**
- All int (0/1) columns use integer dtype, not boolean, for consistent CSV export.
- `yellow_cards` is adjusted: `yellow_cards = yellow_cards - second_yellow_reds` to prevent the triggering yellow in an RSY dismissal from being counted twice.
- `IH` (substituted in at half-time) and `OH` (substituted out at half-time) are included in `sub_in` / `sub_out` respectively; approximately 600 half-time substitution events were captured this way.
- `coach_nationality` is nullable for the small number of coaches whose name string does not contain a parseable 3-letter code in parentheses.

---

## Data Quality Notes

- Missing values present in **Position** and **Shirt Number**
- Event column is sparse (only filled for key events)
- Duplicate records were removed during cleaning
- Same player appears multiple times across matches (expected behavior)
- Some inconsistencies in text formatting were standardized