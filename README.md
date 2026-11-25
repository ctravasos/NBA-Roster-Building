# NBA Roster Building: Expansion Draft Simulation

A statistical approach to building an NBA roster from scratch, applied to a hypothetical expansion draft for Seattle and Las Vegas. Players are ranked with a per-minute efficiency score built from box-score data, and the resulting rosters are compared against the average statistical profile of recent playoff teams.

## The question

If an expansion team could draft from every unprotected player in the league, which 10 players would best replicate the per-game production of a typical playoff team? How does the answer change for a "win now" team compared with a team built around young players?

## Data

- **Source:** BigDataBall NBA game-level box scores, team and player files.
- **Seasons:** 2015-16 through 2023-24, regular season only. Playoff, play-in and In-Season Tournament games were removed.
- **Team data:** opponent, result and team box-score stats per game. Overtime columns were dropped, and only teams that made the playoffs were kept to build target averages.
- **Player data:** position, minutes and standard box-score stats per game.
- Raw data files are not included in this repository; the notebook reads them from local paths.

## Approach

1. **Cleaning.** Combined nine seasons of team and player files, renamed columns and gave each player a single position (his most frequent G/F/C listing).
2. **Rate stats.** Converted every counting stat to per-minute and per-48-minute values so bench players can be compared with starters.
3. **Eligibility filters.** Only games with at least 10 minutes played; only players active in 2023-24 who appeared in at least 41 games that season.
4. **Protected players.** Treated anyone with an All-Star, All-NBA, All-Defensive or All-Rookie selection as protected, as a stand-in for the actual expansion rules (not yet announced).
5. **Three player pools.**
   - *Best possible:* all eligible 2023-24 players, no protection.
   - *"Vegas" (win now):* all unprotected players.
   - *"Seattle" (young core):* unprotected players with no games before the 2021-22 season.
6. **Efficiency score.** Career per-minute averages were min-max normalized for points, field goals, threes, assists, total/offensive/defensive rebounds, steals and blocks (positive) and field goal attempts (negative), then combined with equal weights.
7. **Roster selection.** Took the top 4 guards, 4 forwards and 2 centers by efficiency score. The team's expected game line assumes each player plays 24 minutes, and it is compared with the playoff-team averages.
8. **Visualization.** Tableau dashboards show per-48 leaderboards, draft boards for each team and expected game stats against the target.

## Key findings

- **Target profile.** Playoff teams averaged 111.4 points, 41.0 FG on 87.4 FGA, 11.8 threes, 44.4 rebounds, 24.6 assists, 5.0 blocks, 7.7 steals and 13.2 turnovers per game.
- **Vegas roster:** CJ McCollum, Jordan Clarkson, Jordan Poole, Cole Anthony, Jalen Smith, Paul Reed, Bobby Portis, Trayce Jackson-Davis, Jusuf Nurkic, Day'Ron Sharpe. Expected line: 124.1 points, 48.3 FG on 97.9 FGA, 61.3 rebounds, 22.1 assists, 15.4 turnovers.
- **Seattle roster:** Cam Thomas, Jaden Hardy, Marcus Sasser, Ausar Thompson, Trayce Jackson-Davis, Jalen Johnson, Jonathan Kuminga, GG Jackson, Day'Ron Sharpe, Alperen Sengun. Expected line: 118.7 points, 45.5 FG on 92.2 FGA, 52.7 rebounds, 22.5 assists, 14.7 turnovers.
- **The model favors big men.** Without position quotas it selected mostly centers. Even with quotas, most forwards chosen are power forwards who can play center. Both expansion rosters fall short of the target in threes (9.6 and 9.3 vs. 11.8) and assists, and exceed it in rebounds.
- **Not always the obvious pick.** The unconstrained "best possible" roster included Andre Drummond alongside Shai Gilgeous-Alexander, Stephen Curry, Luka Doncic, Giannis Antetokounmpo and Nikola Jokic. Expected field goal attempts also ran above the target, which suggests the model could penalize shot volume more heavily.

Limitations noted in the write-up: no advanced defensive metrics beyond steals and blocks, no biometrics (height, wingspan, speed, age), no salary data, and no measure of fit, chemistry or experience. The model is meant as one input for a front office, not a final answer.

## Tools

R (R Markdown) with `readxl`, `dplyr`, `tidyr` and `openxlsx` for data preparation and the efficiency model; Tableau for dashboards.

## Repository contents

| File | Description |
|---|---|
| [docs/NBA Roster Building Write Up.pdf](docs/NBA%20Roster%20Building%20Write%20Up.pdf) | Written report: problem statement, methodology, rosters and appendix tables |
| [docs/NBA Roster Building Ppt.pdf](docs/NBA%20Roster%20Building%20Ppt.pdf) | Presentation slides |
| [CVDI2 NBA Project.Rmd](CVDI2%20NBA%20Project.Rmd) | R Markdown source: data cleaning, filters, player pools, efficiency model and roster selection |
| [CVDI2-NBA-Project.html](CVDI2-NBA-Project.html) | Rendered HTML output of the R Markdown notebook |
| [docs/CVDI2-NBA-Project.pdf](docs/CVDI2-NBA-Project.pdf) | Rendered PDF output of the R Markdown notebook |
| [dashboards/PlayerDataA.twbx](dashboards/PlayerDataA.twbx) | Tableau workbook for all eligible 2023-24 players (league leaders, best possible roster) |
| [dashboards/PlayerDataV.twbx](dashboards/PlayerDataV.twbx) | Tableau workbook for the "Vegas" pool of unprotected players (draft board, expected stats) |
| [dashboards/PlayerDataS.twbx](dashboards/PlayerDataS.twbx) | Tableau workbook for the "Seattle" pool of young unprotected players (draft board, expected stats) |
| [dashboards/TeamData.twbx](dashboards/TeamData.twbx) | Tableau workbook for playoff and champion team averages |

The workbook descriptions are based on the datasets the notebook exports (`PlayerDataA`, `PlayerDataV`, `PlayerDataS`, `TeamData`) and the charts shown in the write-up and slides. Open the `.twbx` files in Tableau Desktop or Tableau Reader.
