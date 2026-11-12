# ERCOT Electricity Demand Analysis and Forecasting

## Project Overview

This project explores electricity demand patterns across the Electric Reliability Council of Texas (ERCOT) system using 2025 hourly native-load data. It combines exploratory data analysis (EDA) with machine learning to investigate historical demand patterns and evaluate next-day electricity load forecasting.

The project consists of two complementary Jupyter notebooks:

1. **Exploratory Data Analysis:** Examines hourly, daily, weekly, seasonal, and regional electricity demand patterns.
2. **Electricity Demand Forecasting:** Develops and evaluates machine learning models using calendar features and historical load observations.

The objective is to demonstrate an end-to-end data science workflow, from data preparation and visualization to feature engineering, model selection, and evaluation.

## Dataset

- **Source:** Electric Reliability Council of Texas (ERCOT)
- **Period:** January–December 2025
- **Frequency:** Hourly
- **Target variable:** Total ERCOT electricity demand (MW)
- **Regional data:** Coast, East, Far West, North, North Central, South, South Central, and West

The raw dataset contains 8,761 rows, including the header row, corresponding to 8,760 hourly observations.

The forecasting dataset contains 8,448 observations across 352 complete reporting dates after excluding dates affected by incomplete daily profiles or unavailable historical lag features.

## Project Structure

```text
ERCOT-DEMAND-ANALYSIS/
├── Data/
│   └── Native_Load_2025.xlsx
├── Images/
├── Notebooks/
│   ├── 01_ercot_demand_analysis.ipynb
│   └── 02_ercot_demand_forecasting.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Notebook 01 — Exploratory Data Analysis

The first notebook investigates the characteristics of electricity demand across ERCOT and its regional zones.

Key analyses include:

- Data cleaning and validation, including daylight-saving-time irregularities.
- Hourly electricity demand patterns.
- Weekday versus weekend demand.
- Monthly and seasonal demand variation.
- Regional contributions to total ERCOT load.
- Identification of peak-demand periods.

These findings provide context for the forecasting models developed in the second notebook.

## Notebook 02 — Electricity Demand Forecasting

The second notebook evaluates machine learning models for predicting all 24 hourly ERCOT load observations for the next reporting date.

### Feature Engineering

The forecasting models use:

- Hour ending
- Day of week
- Month
- Previous-day load at the same hour
- Previous-week load at the same hour

### Modeling Approach

The dataset is divided chronologically into training, validation, and test sets, with complete reporting dates kept together.

Models evaluated:

- Previous-day persistence baseline
- Previous-week persistence baseline
- Linear Regression
- Decision Tree Regression
- Random Forest Regression

Decision Tree and Random Forest models were evaluated at multiple maximum-depth settings. The final Random Forest was selected based on validation RMSE before evaluation on the held-out test set.

### Final Test Results

| Model | Test MAE (MW) | Test RMSE (MW) |
|---|---:|---:|
| Previous-day persistence baseline | 2,351.07 | 3,127.62 |
| Tuned Random Forest | 2,325.87 | 3,002.47 |

The tuned Random Forest achieved a **4.00% reduction in test RMSE** and a **1.07% reduction in test MAE** relative to the previous-day persistence baseline.

### Model Interpretation

Exploratory feature analysis found a strong positive correlation between previous-day ERCOT load and the forecasting target (r = 0.94) in the training data.

Feature-importance analysis also identified previous-day load as the dominant predictor in the tuned Random Forest.

Residual analysis and actual-versus-predicted visualizations highlighted areas where the model performed well and where prediction errors remained, particularly around certain high-demand and low-demand periods.

## Limitations

- The dataset covers only one year.
- Weather and holiday predictors were not included.
- Some dates were excluded because of incomplete daily or lagged profiles.
- Forecast issuance assumes that the preceding day's complete load observations are available.
- Hyperparameter tuning used a single chronological validation period rather than expanding-window cross-validation.
- The results have not been validated for operational deployment.

## Future Improvements

Potential extensions include incorporating weather forecasts, using multiple years of data, engineering additional calendar and historical-load features, applying time-series cross-validation, and comparing more advanced forecasting algorithms.

## How to Run

1. Download or clone the repository.
2. Install the dependencies using `pip install -r requirements.txt`.
3. Open the project folder in VS Code or JupyterLab.
4. Run `01_ercot_demand_analysis.ipynb` and `02_ercot_demand_forecasting.ipynb` from the `Notebooks` directory.

Both notebooks expect the dataset at `../Data/Native_Load_2025.xlsx`.

## Tools and Libraries

Python, pandas, NumPy, Matplotlib, Seaborn, scikit-learn, openpyxl, Jupyter Notebook, and VS Code.

## Author

Eunice Adegelu

MS Engineering Data Science and AI — University of Houston
