# FPL H2H

This project is a Fantasy Premier League head-to-head modelling and optimization pipeline. It combines historical player data, expected points modelling, league selection estimation, and optimization routines to generate candidate squads and rankings for FPL head-to-head formats.

The repository is organized around three core ideas:

- Predict player performance and expected points
- Estimate league/selection behaviour and opponent strength effects
- Optimize team selection using those model outputs

---

## Project overview

The project uses a mix of:

- Regression and predictive modelling for player points
- Correlation-based feature filtering
- Bayesian / Dirichlet-style league selection modelling
- Rolling optimization over historical gameweeks
- Stored model artifacts and historical predictions in `.joblib` and CSV outputs

This is a research and experimentation project rather than a packaged production app, so the workflow is driven largely by notebooks and model artifacts stored in the repository.

---

## Repository structure

```text
.
├── README.md
├── data/
│   ├── league_selections_df.csv
│   ├── rolled_data_23_24.csv
│   └── rolled_data_24_25.csv
├── beta estimators/
│   ├── utils.py
│   ├── estimates/
│   ├── league betas/
│   └── opponent betas/
├── predictors/
│   ├── utils.py
│   ├── models_by_round_*
│   ├── hist/
│   └── points_predictor.ipynb
├── optimizer/
│   ├── copula_estimation.ipynb
│   ├── rolling_optimization.ipynb
│   ├── copula_results_all.joblib
│   └── rolling_optimization_results.joblib
└── beta estimators/
    └── ...
```

### Key folders

#### `data/`

Contains the main data sources used by the project, including:

- `league_selections_df.csv`: league selection / market-share style data
- `rolled_data_23_24.csv` and `rolled_data_24_25.csv`: rolled or prepared historical FPL datasets

#### `predictors/`

Contains the player-points prediction pipeline and historical evaluation outputs.

- `points_predictor.ipynb`: model experiments and prediction generation
- `utils.py`: reusable feature selection and model evaluation helpers
- `hist/`: historical prediction outputs and feature files
- `models_by_round_*`: model artifacts stored by gameweek / position

#### `beta estimators/`

Contains the league/opponent modelling work used to estimate selection probabilities and relationships between players and teams.

- `utils.py`: beta regression utilities and pruning helpers
- `league betas/`: league-level modelling notebooks and saved model outputs
- `opponent betas/`: opponent and longitudinal estimation notebooks
- `estimates/`: joblib model artifacts produced by earlier fitting runs

#### `optimizer/`

Contains optimization and copula-related work used to turn predicted values into team selections or strategy recommendations.

- `rolling_optimization.ipynb`: optimization workflow across historical periods
- `copula_estimation.ipynb`: statistical relationship modelling
- `.joblib` files: saved optimization results and fitted objects

---

## Main modelling workflow

The repository follows a staged analytical pipeline:

1. Load and prepare historical FPL data
2. Build expected points / player performance models in `predictors/`
3. Estimate selection and opponent effects in `beta estimators/`
4. Run optimization logic in `optimizer/`
5. Store outputs as CSV and `.joblib` artifacts for reuse

---

## What the code is doing

### Player prediction layer

The predictive layer focuses on expected points for specific positions and player groups. It uses techniques including:

- feature selection based on target correlation
- chronological train/test validation by gameweek
- regressor comparison
- model serialization for later use

The utilities in `predictors/utils.py` show a workflow for:

- selecting informative features
- validating models chronologically
- comparing regressor performance
- saving predictions or model objects

### League selection / beta modelling

The beta estimation pipeline is more probabilistic and uses a Bayesian style approach to model selection distributions. It includes:

- feature pruning using correlation checks
- Dirichlet / beta-style modelling
- HDI-based filtering for significant effects
- refitting on reduced feature sets

This indicates the project is exploring league-level selection and relative attractiveness patterns in a head-to-head context.

### Optimization layer

The optimizer notebooks use rolling optimization and copula-based estimation to search for strong candidate lineups according to the model outputs. Outputs are stored as joblib artifacts rather than being generated on the fly in a single script.

---

## Dependencies

This project depends on Python data science tooling, including packages such as:

- pandas
- numpy
- scipy
- scikit-learn
- matplotlib
- seaborn
- pycaret
- pymc
- arviz
- joblib
- xgboost / lightgbm (where available)

---

## Direction for use:

1. Open the appropriate notebook in `predictors/`, `beta estimators/`, or `optimizer/`
2. Load the relevant CSV or model artifact
3. Run the notebook cells to generate fresh predictions or optimization results
4. Inspect saved outputs in `hist/`, `estimates/`, or the optimizer folder

### Workflow is:

- use the predictor notebooks to generate point estimates
- use beta estimator notebooks to create selection/league influence models
- use optimizer notebooks to produce candidate lineups or ranking signals
