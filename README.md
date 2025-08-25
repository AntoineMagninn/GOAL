# GOAL
Constructing a Global Performance Index for Football Players Based on Weighted and Contextualised Ratings

---

## Repository layout

| Path | Description |
|-------|-------------|
| **GOAL/data/** | Input datasets (read-only in notebooks) |
| ├─ `data.csv` | Raw match-level ratings |
| ├─ `team_rankings.csv` | Team strength and competition rankings |
| └─ `top100guardian.csv` | Guardian Top 100 player rankings |
| **GOAL/notebooks/** | Reproducible pipeline notebooks (run in order) |
| ├─ `Cleaning_EDA.ipynb` | Data cleaning and preprocessing |
| ├─ `Model.ipynb` | Rating model construction and tuning |
| └─ `Answer_analysis.ipynb` | Final analysis and insights |
| **GOAL/results/** | Auto-generated outputs and visualizations |
| ├─ `final_ratings_tuned.csv` | Final tuned ratings per player |
| ├─ `final_ratings_tuned_human_ai.csv` | Ratings for tuned, human-only, and AI-only systems |
| ├─ `player_match_ratings.csv` | Player-match-level ratings with context |
| ├─ `player_summary.csv` | Summary stats merged with ratings |
| ├─ `ratings_timeseries_tuned.csv` | Match-by-match tuned ratings |
| ├─ `ratings_timeseries_tuned_human_ai.csv` | Time series for all rating systems |
| ├─ `top10_by_system.csv` | Leaderboards for each system |
| ├─ `mechanics_eric_m*.png` | Visualizations explaining update mechanics |
| ├─ `mustafi_ratings_ex*.png` | Examples of rating updates |
| ├─ `top_features_human*.png` | SHAP feature importance for human ratings |
| └─ `top_features_ai*.png` | SHAP feature importance for AI ratings |
| **GOAL/.gitignore** | Ignore rules for version control |
| **GOAL/README.md** | This file |

All notebooks auto-detect the GOAL root, so **no manual path editing is required**.

---

## Pipeline Overview

### **1) Cleaning_EDA.ipynb**
- **Loads:** `data/data.csv`, `data/team_rankings.csv`
- **Process:**  
  - Cleans and standardizes ratings across raters  
  - Mean-centering per rater  
  - Aggregates to one row per `(player, match)`  
  - Adds opponent difficulty from rankings  
- **Outputs:**  
  - `results/player_match_ratings.csv`  
- **Extras:** Exploratory plots and statistics for data validation  

---

### **2) Model.ipynb**
- **Loads:**  
  - `results/player_match_ratings.csv`  
  - `data/top100guardian.csv`
- **Process:**  
  - Builds three systems: **human-only**, **AI-only**, and **tuned (hybrid)**  
  - Applies context-aware Elo-like updates to create time series  
  - Runs grid search to tune parameters against Guardian Top 100 rankings  
- **Outputs:**  
  - `results/final_ratings_tuned.csv`  
  - `results/final_ratings_tuned_human_ai.csv`  
  - `results/ratings_timeseries_tuned.csv`  
  - `results/ratings_timeseries_tuned_human_ai.csv`  
  - `results/top10_by_system.csv`  
- **Extras:**  
  - Visualizations saved as PNGs (`mechanics_eric_m*.png`, `mustafi_ratings_ex*.png`)

---

### **3) Answer_analysis.ipynb**
- **Loads:**  
  - `results/player_match_ratings.csv`  
  - `results/final_ratings_tuned_human_ai.csv`
- **Process:**  
  - Builds per-player summaries (including per-minute stats and positions)  
  - Runs SHAP feature importance analysis  
  - Compares human vs AI drivers  
- **Outputs:**  
  - `results/player_summary.csv`
- **Extras:**  
  - SHAP plots saved as `top_features_human*.png` and `top_features_ai*.png`

---