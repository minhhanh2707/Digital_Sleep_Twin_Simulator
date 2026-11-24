# Digital_Sleep_Twin_Simulator
A data-driven digital twin that predicts sleep quality from lifestyle factors and simulates what-if scenarios by conducting data cleaning, exploratory analysis, correlation assessment, preprocessing, trainning and comparing two machine learning models Linear Regression and Random Forest.

## Overview

Using the Kaggle **Sleep Health and Lifestyle** dataset (374 rows), I trained regression models to
predict **Quality of Sleep (1–10)**, then wrapped the best model in a `simulate()` function. The
function takes one person's profile, changes one or more habits, and reports how the predicted
sleep quality moves. Treating a single person's profile as a "twin" that can be experimented on is
the core idea.

## Key Results

| Model | R² | MAE (quality points) |
|---|---|---|
| Linear Regression | 0.966 | 0.120 |
| Random Forest | **0.987** | **0.041** |

- **Stress Level** (r ≈ −0.90) and **Sleep Duration** (r ≈ +0.88) are the strongest drivers of sleep quality.
- Example twin (27-year-old male software engineer, baseline predicted quality **5.88**):

| Scenario | Predicted quality | Change |
|---|---|---|
| Stress −2 | 6.58 | +0.70 |
| Sleep +1 hour | 7.19 | +1.31 |
| Heart rate +3 | 5.44 | −0.43 |
| Combined: stress −3, sleep +1 h, activity +30 | 7.47 | **+1.59** |

- The same intervention does not help everyone equally: lowering stress by 2 gave the first twin
  +0.70, but a high-stress twin only +0.15.
## Method

1. **Data cleaning:** merged the duplicate `Normal` / `Normal Weight` BMI labels, split blood pressure into `Systolic` and `Diastolic`, dropped the ID column.
2. **EDA and correlation analysis:** distributions, categorical balance, sleep quality by group, correlation heatmap.
3. **Hypotheses stated up front,** then checked against the data.
4. **Modeling:** scikit-learn `Pipeline` with `StandardScaler` (numeric) and `OneHotEncoder` (categorical), an 80/20 split, then Linear Regression vs. Random Forest.
5. **Digital twin:** `predict_quality()` and `simulate()` helpers, single and combined what-if scenarios, and lever sweeps across a feature's full range.

## Limitations

- **Associations, not causation**: The model reflects patterns in observational data. It cannot prove
  that changing a habit will change sleep.
- **Occupation imbalance**: predictions for rare occupations are largely extrapolation.
- **Not medical advice.**: This is a demonstration of the digital-twin methodology.

## Repository Structure

```
sleep-digital-twin/
├── notebooks/
│   └── sleep_digital_twin.ipynb   # full analysis, answers and simulations
├── data/
│   └── README.md                  # how to get the dataset
├── images/                        # figures used in this README
├── requirements.txt
├── LICENSE
└── README.md
```

## How to Run

1. Download the **Sleep Health and Lifestyle Dataset** from Kaggle and save the CSV as
   `data/Sleep_health_and_lifestyle_dataset.csv` (see `data/README.md`).
2. Then:

```bash
git clone https://github.com/<your-username>/sleep-digital-twin.git
cd sleep-digital-twin
pip install -r requirements.txt
jupyter notebook notebooks/sleep_digital_twin.ipynb
```

## Future Work

- Prediction intervals for the twin's predictions
- An interactive `ipywidgets` demo with sliders
- Gradient boosting (XGBoost / LightGBM) comparison
- A group-aware train/test split and validation of the counterfactuals against sleep-science literature

## Tech Stack

Python · pandas · NumPy · scikit-learn · matplotlib · seaborn · Jupyter

## Acknowledgments

Completed as part of the **National Student Data Corps (NSDC)** program.
Dataset: *Sleep Health and Lifestyle Dataset* on Kaggle.
