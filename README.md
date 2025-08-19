# GOAL
Constructing a Global Performance Index for Football Players Based on Weighted and Contextualised Ratings

## Repository layout
```
GOAL/
├─ data/                # Input datasets (read-only in notebooks)
│  ├─ data.csv
│  ├─ team_rankings.csv
│  └─ top100guardian.csv
├─ notebooks/           # Reproducible pipeline (run in order)
│  ├─ Cleaning_EDA.ipynb
│  ├─ Model.ipynb
│  └─ Answer_analysis.ipynb
├─ results/             # All generated outputs (created/overwritten by notebooks)
│  ├─ player_match_ratings.csv
│  ├─ final_ratings_tuned.csv
│  ├─ final_ratings_tuned_human_ai.csv
│  ├─ ratings_timeseries_tuned.csv
│  ├─ ratings_timeseries_tuned_human_ai.csv
│  ├─ top10_by_system.csv
│  └─ player_summary.csv
└─ README.md
```

Path handling:
- All notebooks auto-detect the GOAL root and always:
  - read inputs from GOAL/data
  - write outputs to GOAL/results
No manual path edits are required.

## Run order (pipeline)
1) Cleaning_EDA.ipynb
- Loads: data/data.csv, data/team_rankings.csv
- Cleans and standardizes ratings (handles different rater scales, mean-centering per rater)
- Aggregates to one row per (player, match) with both human and non-human means
- Adds opponent information and difficulty from team_rankings
- Saves: results/player_match_ratings.csv
- Produces some exploratory plots/examples

2) Model.ipynb
- Loads: results/player_match_ratings.csv, data/top100guardian.csv
- Builds three systems:
  - human-only, non-human-only, and a “tuned” mixed system
- Computes opponent-difficulty–aware ratings time series via an Elo-like update
- Runs a small grid search to tune parameters against an external ranking subset (Guardian list)
- Saves:
  - results/final_ratings_tuned.csv
  - results/ratings_timeseries_tuned.csv
  - results/final_ratings_tuned_human_ai.csv (merged tuned/human/ai finals)
  - results/ratings_timeseries_tuned_human_ai.csv
  - results/top10_by_system.csv
- Includes diagnostics and plots

3) Answer_analysis.ipynb
- Loads: results/player_match_ratings.csv, results/final_ratings_tuned_human_ai.csv
- Builds per-player summary with per-minute stats and primary position
- Runs SHAP-based feature importance analyses by position
- Compares AI vs Human drivers (e.g., “Top 20 most different features”)
- Saves: results/player_summary.csv

## Quick start
- Recommended Python: 3.10+ (tested with 3.12)
- Create a virtual environment and install dependencies:
  ```
  python3 -m venv .venv
  source .venv/bin/activate
  pip install -U pip
  pip install pandas numpy scikit-learn matplotlib shap
  ```
- Open the GOAL folder in VS Code, select the .venv kernel for notebooks.
- Run the notebooks in the order listed above. Outputs will appear under results/.

## Data files
- data.csv: raw ratings at match level. Expected columns include player, match, date, rater, is_human, original_rating, minutesPlayed, pos, team, competition, etc.
- team_rankings.csv: opponent strength (columns: team, competition, ranking).
- top100guardian.csv: Guardian Top 100 list with at least columns Name, Rank, Domestic league.

## Outputs (key)
- player_match_ratings.csv: aggregated player-match table with human/non-human mean ratings + context.
- final_ratings_tuned.csv: final tuned rating per player.
- final_ratings_tuned_human_ai.csv: final ratings for tuned, human-only, AI-only.
- ratings_timeseries_tuned.csv: tuned rating trajectory per match.
- ratings_timeseries_tuned_human_ai.csv: stacked time series for all three systems.
- player_summary.csv: per-minute stat summary merged with final ratings; used for SHAP analyses.
- top10_by_system.csv: leaderboard snapshots.

## Re-running the pipeline
- If you change anything in data/, re-run notebooks in order to regenerate results/.
- Notebooks overwrite existing files in results/ to keep outputs in sync.