# EDGE Pass Rusher Evaluation Framework
### 2025 NFL Big Data Bowl

This project builds a multi-dimensional evaluation framework for EDGE pass rushers using NFL player tracking data from the 2025 Big Data Bowl competition. I classify each rush attempt by technique (speed, power, counter, or loss), compute a context-adjusted pressure rate that accounts for blocking difficulty, and produce player rankings with scheme-fit tiers.

## Project Overview

Traditional pass rush stats like sacks and hurries do not capture how a pass rusher actually wins individual matchups. This framework goes deeper by:

- Classifying each rush attempt by win type using player tracking data
- Adjusting pressure rates for blocking context (single-blocked vs. double-teamed)
- Ranking players by technique profile to help identify scheme fits

## Setup

### 1. Create a virtual environment and install dependencies

Run the following from the `nfl-big-data-bowl-2025/` directory:

```
python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # Mac/Linux
pip install -r requirements.txt
```

If you are using VS Code, select the `.venv` interpreter after activating it (Ctrl+Shift+P, then "Python: Select Interpreter").

### 2. Get the data

Download the dataset from Kaggle:
https://www.kaggle.com/competitions/nfl-big-data-bowl-2025/data

Unzip the downloaded files into the `data/` folder. The final folder structure should look like this:

```
nfl-big-data-bowl-2025/
├── data/
│   ├── games.csv
│   ├── plays.csv
│   ├── players.csv
│   ├── player_play.csv
│   ├── tracking_week_1.csv
│   ├── tracking_week_2.csv
│   │   ...
│   └── tracking_week_9.csv
├── notebooks/
└── outputs/
```

The CSV files are not committed to this repo due to file size. Generated outputs in `outputs/` are also excluded from version control.

### 3. Run the notebooks in order

Open each notebook in VS Code (Jupyter extension required) and run all cells top to bottom. Each notebook depends on the outputs of the one before it, so the order matters.

| Notebook | What it does | Output |
|---|---|---|
| `01_data_pipeline.ipynb` | Load, filter, standardize, and clip tracking data | `outputs/tracking_clipped.parquet`, `outputs/edge_player_play.csv` |
| `02_getoff_burst.ipynb` | Get-off time and burst speed per rush attempt | `outputs/getoff_metrics.csv` |
| `03_blocker_metrics.ipynb` | Blocker separation, displacement, hip turn, and blocking context | `outputs/blocker_metrics.csv` |
| `04_win_type_classification.ipynb` | Win type classification and context-adjusted pressure rate | `outputs/win_types.csv`, `outputs/player_win_type_summary.csv` |
| `05_model_validation.ipynb` | ROC-AUC validation and player rankings with technique indices | `outputs/edge_rush_player_rankings.csv` |
| `06_visualizations.ipynb` | Technique profiles, quadrant scatter, double-team rate, and player rankings | `outputs/figures/` |

## Dataset

**Source:** [NFL Big Data Bowl 2025](https://www.kaggle.com/competitions/nfl-big-data-bowl-2025/data)

**Season:** 2022 NFL season, Weeks 1 through 9

**Scope:** Passing plays only (`isDropback == True`); EDGE rushers (`DE`, `OLB`) with `wasInitialPassRusher == True`

**Tracking:** 10 frames per second with player position, speed, orientation, and direction
