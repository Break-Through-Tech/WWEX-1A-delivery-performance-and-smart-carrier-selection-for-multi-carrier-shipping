# Delivery Performance & Smart Carrier Selection for Multi-Carrier Shipping

Break Through Tech AI Studio — Fall 2026 · Host company: **WWEX (Worldwide Express)**

Predicting realized shipment transit time, and the gap between what carriers
promise and what they actually deliver, while improving carrier selection.

## 👥 Team Members

| Name | GitHub Handle | Contribution |
|---|---|---|
| Tejas Lalwani | [@Tejaslalwani24](https://github.com/Tejaslalwani24) | Train/validation protocol and baseline (#4); feature engineering and model comparison (#7); carrier recommendation; run calibration and fairness checks |
| Hannah Mathew | [@hmathew](https://github.com/hmathew) | Address missing values and outliers; error analysis by mode, lane, and carrier (#6); README completion; run calibration and fairness checks |
| Evelyn Fang | [@fang-evelyn](https://github.com/fang-evelyn) | Address missing values and outliers; error analysis by mode, lane, and carrier (#6); quantile "delivery-window" prediction; run calibration and fairness checks |
| Sargun Bhatia | [@SargunB](https://github.com/SargunB) | Onboard to the shipping domain; feature engineering and model comparison (#7); run calibration and fairness checks |
| Andrew Skoblov | [@andrewskoblov](https://github.com/andrewskoblov) | Train/validation protocol and baseline (#4); build the on-time risk classifier; README completion; run calibration and fairness checks |
| Chiagoziem Onwubuya | [@altairify](https://github.com/altairify) | Onboard to the shipping domain; build the on-time risk classifier; quantile "delivery-window" prediction; run calibration and fairness checks |

**Challenge Advisor:** Arjun Srinivasan, SVP AI & Data Science, WWEX ([@arjunsr82](https://github.com/arjunsr82))
**AI Studio Coach:** Ayush Amberkar

## 🎯 Project Highlights

- Built a gradient-boosted transit-time model (XGBRegressor, absolute-error objective) that predicts realized delivery time in days, then extended it into a cost/speed carrier recommendation engine.
- MAE 0.537 days against the naive promised-days baseline of 0.841, a 36% reduction in average error, with 87.7% of predictions within ±1 day (RMSE 0.758).
- Validation uses a forward-in-time split, so the score reflects predicting future shipments rather than interpolating among known ones.
- Compared six model families (Ridge, Random Forest, Extra Trees, HistGradientBoosting, LightGBM, XGBoost) and tested engineered calendar, size, and interaction features. They gave no lift beyond the ~0.002-day noise floor, so the final model uses the original features.
- Cleaned the dataset’s seeded quality problems: duplicate rows, missing and unit-error weights (grams entered as pounds), whitespace and casing noise in carrier and service fields, and malformed ZIP codes. Imputation medians were computed on train only to prevent leakage.
- Error analysis by carrier, service level, lane, and month showed Falcon Post is hardest to predict (MAE ~0.65 vs ~0.41 for Summit Express).
- Built a counterfactual carrier recommender that predicts transit time and cost for each eligible carrier on the same lane and service, scores each with cost + $8 × predicted days, and recommends a switch only when the improvement exceeds $5. Recommendations are model-based estimates, not verified outcomes.

## 👩🏽‍💻 Setup and Installation

```bash
git clone https://github.com/Break-Through-Tech/WWEX-1A-delivery-performance-and-smart-carrier-selection-for-multi-carrier-shipping.git
cd WWEX-1A-delivery-performance-and-smart-carrier-selection-for-multi-carrier-shipping
pip install -r requirements.txt
```

Then open `WWEX_1A.ipynb` and run the cells in order. The data ships with the
repository in `data/` (`train.csv`, `test.csv`), so no separate download is
needed. ZIP columns must be read as strings or leading zeros are lost; the
loading cell does this already.

Dependencies: `pandas`, `numpy`, `scikit-learn`, `seaborn`, `matplotlib`.

## 🏗️ Project Overview

WWEX is a third-party logistics provider coordinating parcel and LTL
(less-than-truckload) freight across multiple carriers. In multi-carrier
shipping there is a persistent gap between a carrier's published delivery
promise and its realized transit performance. Merchants quote the promise;
customers experience the reality.

Our objective is to predict each shipment's actual transit time, and its
on-time risk, well enough to sharpen merchant-facing delivery estimates and
to support routing shipments to the carrier offering the best real cost-versus-
speed trade-off.

The success bar is set by the carriers themselves: beat the naive strategy of
simply repeating the promised date, which carries an MAE of roughly 0.84 days,
by a meaningful margin that holds up across shipment modes and lanes.

## 📊 Data Exploration

**Dataset:** ShipStation Global Multi-Carrier Shipment Dataset, synthetic and
fully reproducible by seed. No real customer or carrier data.

**Size:** 40,030 training rows and 10,120 test rows, 31 features, spanning 6
carriers, 6 service levels, and 306 origin-to-destination lanes. Shipments run
from 2024-01-01 to 2025-12-30.

**Preprocessing.** The dataset carries deliberate quality defects, each handled
in the loading cell:

- Casing and whitespace noise in `carrier` and `service_level`, normalized
  against a canonical spelling list.
- Missing and unit-error values in `weight_lbs` and the dimension columns.
- Malformed ZIPs, preserved as strings so leading zeros survive.
- Duplicate rows.

**EDA findings.** Transit days are right-skewed and concentrated on a small
number of integer values. Service level separates transit time far more
cleanly than raw distance does: the `Overnight` and `TwoDay` tiers hold their
times across lane lengths, while `Ground` and the LTL tiers spread widely.
Carriers differ in both average transit time and on-time rate, which is what
makes a cost-versus-speed recommendation meaningful. Shipping month and day of
week both move average transit time, pointing at seasonality and weekend
backlog as real signal.

## 🧠 Model Development

**Validation protocol.** Shipments are sorted by ship_date and split forward in time, with the earlier ~80% of days for training and the later ~20% for validation. The cut is made on a date boundary rather than a row index: cutting by row position allowed a single calendar day to land on both sides of the split, and same-day shipments share weather, carrier backlog, and holiday conditions. An assertion in the split cell fails the notebook if any day appears in both sets.

Training covers 2024-01-01 to 2025-08-04 (32,020 shipments); validation covers
2025-08-05 to 2025-12-30 (8,010 shipments).

**Feature set and leakage controls.** Columns dropped from the feature set: the target itself, delivered_on_time (which reveals it), carrier_cost_usd (billed cost is not known at ship time), identifiers, and the raw date and ZIP columns. Median imputation, scaling, and one-hot encoding are fitted on the training split only, inside a pipeline, so no validation information leaks into preprocessing.

**Model.** Six model families were compared on the same forward-in-time split. The first model built was HistGradientBoostingRegressor (loss="absolute_error"), chosen to optimize the metric the project is scored on directly. XGBoost later superseded it as the best performer.

| Model |  Validation MAE (days) |
|---|---|---|
| Promised days (baseline) |	0.841 |
| Ridge	| ~0.59 |
| Extra Trees / Random Forest |	~0.57 |
| HistGradientBoosting | 0.544 |
| LightGBM | 0.542 |
| XGBoost	| 0.537 |

**Feature engineering.** Calendar, log-transform, dimensional-weight, and interaction features were tested against the original feature set. They gave no meaningful lift (XGBoost MAE 0.5366 original vs. 0.5373 engineered), so the final model uses the original features.

**Hyperparameter tuning.** Randomized search with time-series cross-validation was run for XGBoost, LightGBM, and HistGradientBoosting. None beat the hand-set XGBoost (tuned XGBoost 0.5374 vs. 0.5366). Differences are within the ~0.002-day noise floor, so further tuning was not pursued.

**Feature importance.** Permutation importance shows promised_days dominates, followed by carrier, distance_miles, weather_severity, ship day of week, and carrier_congestion_index. Package dimensions and handling flags contribute little.

**Error analysis.** Errors were sliced by shipment mode, carrier, service level, lane, and ship month. Falcon Post is the hardest carrier to predict (MAE ~0.65 vs. ~0.41 for Summit Express). This breakdown was run on the HistGradientBoosting model, so exact figures may differ slightly for XGBoost.

## 📈 Results & Key Findings

| Metric | Naive baseline | Model |
|---|---|---|
| MAE (days) | 0.841 | **0.546** |
| RMSE | — | 0.775 |
| Within ±1 day | — | **86.6%** |

The model reduces average error by about 35% against the promised-date
baseline, clearing the project's September bar. Nearly seven shipments in
eight land within a day of the prediction.

The naive baseline's 0.841 MAE is itself the headline business finding: the
carriers' own published promises are off by roughly a day on average, which is
the gap this project exists to close.

## 🚀 Next Steps

- **On-time risk classifier (Track B).** Predict `delivered_on_time` directly,
  evaluated with ROC-AUC and PR-AUC rather than accuracy, since the classes are
  roughly 73/27.
- **Feature engineering and model comparison (Track A).** Lane-level and
  carrier-level aggregates, and a comparison against alternative regressors.
- **Error analysis** by shipment mode, lane, and carrier, to find where the
  model is weakest rather than reporting a single pooled number.
- **Stretch:** counterfactual carrier recommendation scoring every eligible
  carrier on cost versus speed; quantile regression for calibrated P50–P90
  delivery windows instead of a point estimate.

**Known limitations.** The model is validated on one forward-in-time split, so
the estimate carries no variance bound. Performance is reported pooled, so a
lane or carrier where it fails badly would currently be invisible. The data is
synthetic, which means real-world distribution shift is untested.

## 📝 License

To be selected with the Challenge Advisor's approval.

## 🙏 Acknowledgements

Thanks to Arjun Srinivasan at WWEX for advising this project, and to Ayush
Amberkar for coaching, along with the Break Through Tech AI Studio program.
