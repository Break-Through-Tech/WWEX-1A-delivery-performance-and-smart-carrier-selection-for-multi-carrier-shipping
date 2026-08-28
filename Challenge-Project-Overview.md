# Delivery Performance & Smart Carrier Selection for Multi-Carrier Shipping

**Company / Org:** WWEX   
**Challenge Advisor:** Arjun Srinivasan (SVP - AI & Data Science, `arjun.srinivasan@wwex.com`, GitHub: `@arjunsr82`)  
**AI Studio Coach:** Ayush Amberkar, ayush.amberkar@breakthroughtech.org      
**Program:** Break Through Tech AI Studio - Fall 2026  

---

## 🏢 About WWEX
WWEX (Worldwide Express) is a leading third-party logistics (3PL) provider that helps businesses manage parcel and LTL (Less-Than-Truckload) freight shipping[cite: 31]. In multi-carrier shipping environments, there is often a significant disconnect between carriers' published delivery promises and their actual, realized transit performance[cite: 31, 32]. By leveraging machine learning on historical shipping performance data, WWEX aims to sharpen delivery estimates for merchants and dynamically route shipments to the optimal carrier based on real-world cost vs. speed trade-offs[cite: 31, 32].

---

## 🎯 The Challenge

### Project Summary

In this project, you will use synthetic multi-carrier shipment data (parcel and LTL freight records with lane, carrier, service-level, weight/dimension, seasonality, weather, and congestion features) and regression, classification, and recommendation techniques (gradient-boosted trees, quantile regression, and counterfactual scoring) to predict each shipment's realized delivery transit time and its on-time risk, and recommend the best carrier by cost-versus-speed trade-off. This will help our company address the gap between carriers' published delivery promises and their actual performance — sharpening merchant-facing delivery estimates and improving carrier selection.

### Success Criteria
Primary metric is Mean Absolute Error (in days) on the withheld transit_days, beating the naive promised-date baseline (~0.84 MAE) by a meaningful margin and holding up across modes and lanes. Secondary: RMSE and share of predictions within ±1 day. For on-time risk, ROC-AUC and PR-AUC (not accuracy alone, since classes are ~73/27). A successful December outcome is a reproducible model that clears the baseline, an honest error analysis showing where it's weakest, and a stakeholder-ready read on whether it could be trusted inside a merchant-facing delivery estimate. A reference scoring script (evaluate.py) is provided so results are measured consistently.

### Stretch Goals
(1) Smart carrier recommendation - combine the transit and cost models to score every eligible carrier counterfactually and recommend the best cost/speed trade-off under a service constraint.   
(2) Quantile / delivery-window prediction (e.g., P50–P90) instead of a single point estimate, for calibrated customer promises.    
(3) A standalone cost/rate-prediction model (carrier_cost_usd is included).    
(4) Calibration and fairness analysis across lanes, modes, and regional-vs-national carriers.   

### Project Milestones
| Month | Milestone | Key Activities |
|---|---|---|
| September | Domain Onboarding, EDA & Baseline | Onboard to the shipping domain and dataset; complete EDA; find and fix the deliberate data-quality issues (duplicate rows, missing and unit-error weights, categorical/whitespace noise, malformed ZIPs); establish a clean train/validation protocol and a first transit-time baseline that beats the naive "predict the carrier's promised date" bar (MAE ≈ 0.84 days). |
| October | Feature Engineering & Model Comparison | Feature engineering and model comparison for transit prediction (Track A); build the on-time-risk classifier (Track B), evaluated with ROC-AUC / PR-AUC given the ~73/27 class balance; begin error analysis by shipment mode, lane, and carrier. |
| November | Stretch Goals, Validation & Delivery | Take on a stretch goal: carrier recommendation via counterfactual cost/speed scoring and/or quantile "delivery-window" prediction; run calibration and fairness checks; finalize the technical report and stakeholder presentation. |

---

## 📊 Dataset

**Name and Source:** ShipStation Global Multi-Carrier Shipment Dataset (synthetic) — generated programmatically; no real customer, carrier, or operational data. Fully reproducible by seed.

**Format:** CSV / TSV (train.csv, test.csv)   

**Size:** ~10.4 MB total (≤ 1 GB) — 40,030 training rows and 10,120 test rows, 31 features across 6 carriers, 6 service levels, and 306 origin→destination lanes; two withheld prediction targets.  

**Location:** data/data_dictionary.md   

---

## 🛠️ Suggested Approach

**ML Problem Type:** Classification,Regression,Recommendation Systems 

**Recommended Libraries:**
- pandas / NumPy — data loading, cleaning, feature engineering
- scikit-learn — baselines and core models (HistGradientBoostingRegressor, classifiers, pipelines, metrics)
- XGBoost or LightGBM (optional) — stronger gradient boosting for the transit and cost models
- matplotlib / seaborn — EDA and error-analysis visualizations
  
**Evaluation Metrics:**
- MAE (mean absolute error, in days) — primary metric for transit-time regression (Track A); beat the naive promised-date baseline of ~0.84
- RMSE and % within ±1 day — secondary regression metrics
- ROC-AUC and PR-AUC — on-time classification (Track B), since classes are ~73/27 and accuracy alone misleads
- Cost/speed trade-off objective — for the carrier-recommendation stretch (e.g., cheapest option meeting a service promise at ≥90% confidence)

---

## 📚 Resources to Get Started

The following resources will help your team understand the problem space and potential technical approaches for this project:

**Background Reading:**
- [e.g., Link to an article or blog post about the problem domain]
- [e.g., Link to an industry report or case study]

**Technical Tutorials:**
- [e.g., Link to a free tutorial on the ML technique(s) involved]
- [e.g., Link to documentation for a key library or tool]

**Code Examples:**
- [e.g., Link to a relevant GitHub repo]
- [e.g., Link to a sample implementation or starter code]

**Other:**
- [Links to any additional resources — e.g., papers, videos, podcasts, etc.]

*Feel free to explore beyond these, and share anything interesting you find with me!*

---

## 🤝 How We'll Work Together

**Official check-ins:** During our biweekly 45-minute AI Studio Lab Section meeting block (2nd and 4th week of every month)



 **Other ways to reach out to me with questions:**
Discord, Email (arjun.srinivasan@wwex.com)     
Note: I will aim to respond within 48 hours. Please reach out to your AI Studio Coach with urgent questions.


> 

**Recommended free coding / collaboration tools**
* […]
* […]

---

## 🚀 Getting Started

1. **Review this overview document** and note any questions for our first meeting
2. **Begin reviewing the dataset** using the link above
3. **Read the GitHub Projects documentation** [here](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)

I’m excited to work with you!

---

## ❓ Questions?

Please bring any questions to our first meeting during the week of August 24th (Break Through Tech’s Bridge to Studio - Session C). 
