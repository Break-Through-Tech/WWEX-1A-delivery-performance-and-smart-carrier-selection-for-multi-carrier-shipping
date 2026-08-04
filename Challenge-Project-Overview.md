---

> ## Challenge Advisor: Update & Finalize Your Project Overview
>
> > 💡 **These grey text instructions are just for you, the team's Challenge Advisor; please delete them once you have completed the steps below.**
>
> We've pre-populated this Challenge Project Overview page — which is what will be shared with your Break Through Tech student team in August — using the details from your submission form. You should have received an email inviting you to join this repo as a Collaborator, enabling you to add files and make edits.
> 
> In order for your project to be finalized and assigned to a team, please:
> 1. **Review all sections below** and update or expand any content as needed, making sure to address the SME Feedback in the section immediately below. Look for square brackets to find the places below that require additional inputs from you (e.g., "About [Company / Org Name]").
> 2. **Add your dataset** to the [data folder](data) in this repo.
> 3. **Close the Issue assigned to you in this repo** to let us know that you have made your edits and the overview page is ready for final review. You can do this by going to the _Issues_ tab in the top left section of the menu above, add a comment that says "CA review complete", and click the button to Close the Issue. 
>
> If you're unfamiliar with how to edit a page like this in GitHub, check out [this tutorial](https://ubc-lib-geo.github.io/gis-workshop-waml-template/content/handson/edit-readme.html) for a quick overview (start with step 2 and only edit this page), and [this guide](https://ubc-lib-geo.github.io/gis-workshop-waml-template/content/markdown.html) on how to use Markdown to compose text.
>
>
> ❌ Remember that this is a public repo. Do NOT include: Proprietary data, PII, API keys, credentials, or anything confidential.

---

### 🔍 SME Feedback from the Break Through Tech Evaluation Team

## 📋 BTT Internal Evaluation Notes
*(This section is for BTT staff and CAs only — remove before sharing with students)*

### Technical Vetting
| Check | Status | Notes |
| :--- | :--- | :--- |
| Python Compatibility | 🟢 | 100% Python-native stack (`pandas`, `numpy`, `scikit-learn`, `xgboost` / `lightgbm`)[cite: 31, 42]. Fits standard tabular ML workflows. |
| Data Readiness | 🟢 | CA-provided synthetic dataset (< 1 GB)[cite: 31, 54, 61]. Includes a data dictionary and intentional real-world data quality issues for student cleaning exercises[cite: 42, 62, 65]. |
| Resource Check | 🟢 | Executable entirely within the Google Colab free tier. Lightweight tabular dataset avoids memory crashes. |

### Internal Scores
- **Student Fit Score:** 9/10 (Difficulty rating 5.5/10 — ideal "sweet spot" for undergraduate fellows post-ML Foundations).
- **Technical Depth Score:** 9/10 (Spans full end-to-end ML lifecycle: EDA/data cleaning, regression modeling, imbalanced classification, counterfactual recommendation scoring, and fairness/calibration analysis)[cite: 31, 42].
- **Overall Recommendation:** APPROVE AS IS

### Advisor Feedback Draft
This is an outstanding corporate challenge proposal from WWEX that perfectly aligns with Break Through Tech's pedagogical goals[cite: 10, 31]. The project offers a structured, multi-track workflow that moves fellows from foundational tabular data cleaning up to gradient boosting, quantile regression, and counterfactual carrier recommendation[cite: 31, 42].

**Key Advisor Guardrails:**
1. Enforce **PR-AUC** and **ROC-AUC** (rather than raw accuracy) for the on-time risk classifier due to the ~73/27 class imbalance[cite: 42].
2. Require time-based validation splits to prevent future performance data leakage into past transit predictions.
3. Use the provided reference evaluation script (`evaluate.py`) to maintain consistent metric tracking across all project phases[cite: 42].

---

# Delivery Performance & Smart Carrier Selection for Multi-Carrier Shipping

**Company / Org:** WWEX   
**Challenge Advisor:** Arjun Srinivasan (SVP - AI & Data Science, `arjun.srinivasan@wwex.com`, GitHub: `@arjunsr82`)  
**Program:** Break Through Tech AI Studio - Fall 2026  

---

## 🏢 About WWEX
WWEX (Worldwide Express) is a leading third-party logistics (3PL) provider that helps businesses manage parcel and LTL (Less-Than-Truckload) freight shipping[cite: 31]. In multi-carrier shipping environments, there is often a significant disconnect between carriers' published delivery promises and their actual, realized transit performance[cite: 31, 32]. By leveraging machine learning on historical shipping performance data, WWEX aims to sharpen delivery estimates for merchants and dynamically route shipments to the optimal carrier based on real-world cost vs. speed trade-offs[cite: 31, 32].

---

## 🎯 The Challenge

### Project Summary
In this project, you will work with synthetic multi-carrier shipment data covering parcel and LTL freight records. You will apply regression, classification, and recommendation techniques (including gradient-boosted trees, quantile regression, and counterfactual scoring) to:
1. [cite_start]**Predict Realized Transit Time (Track A):** Estimate actual delivery transit days and beat naive carrier-promised benchmarks.
2. [cite_start]**Predict On-Time Risk (Track B):** Classify whether a shipment is at risk of being delayed under imbalanced class conditions (~73/27 split).
3. [cite_start]**Smart Carrier Recommendation (Track C):** Recommend the best carrier by evaluating counterfactual cost-versus-speed trade-offs under service-level constraints.

### Success Criteria
- **Transit Time Baseline:** Beat the naive "carrier's promised date" baseline (~0.84 days Mean Absolute Error) by a meaningful margin on held-out test data.
- **Prediction Accuracy:** Maximize Root Mean Squared Error (RMSE) performance and the proportion of transit predictions within $\pm 1$ day of actual delivery.
- **On-Time Risk Classification:** Achieve strong PR-AUC and ROC-AUC scores for delay predictions.
- **Calibration & Fairness:** Demonstrate stable calibration and model fairness across geographic lanes, shipment modes (parcel vs. LTL), and carrier types (regional vs. national).
- **Reproducible Harness:** Deliver a standardized scoring pipeline using `evaluate.py` to benchmark model performance.

### Project Milestones
| Month | Milestone | Key Activities |
| :--- | :--- | :--- |
| **September** | Domain Onboarding, Data Cleaning & Baseline Protocol | Onboard to shipping domain concepts. Conduct EDA to identify and fix deliberate data-quality issues (duplicate rows, unit conversion errors, missing weights, whitespace noise, malformed ZIPs). Establish clean train/validation protocol and build a first transit-time regression baseline aiming to beat ~0.84 MAE. |
| **October** | Feature Engineering & Dual-Track Modeling | Engineer lane, carrier, weight/dimension, seasonality, weather, and congestion features. Build transit-time regression models (Track A) and on-time risk classifiers (Track B) evaluated with ROC-AUC and PR-AUC. Begin systematic error analysis across modes, lanes, and carriers. |
| **November** | Counterfactual Recommendation & Model Calibration | Combine transit and cost models to score eligible carriers counterfactually. Build smart carrier recommendations optimizing cost/speed trade-offs under service constraints. Run calibration and fairness checks across regional/national carriers and finalize technical documentation. |
| **December** | Evaluation, Error Analysis & Final Deliverables | Run final benchmark evaluations using `evaluate.py`. Perform qualitative error analysis detailing model limitations and readiness for merchant-facing delivery estimates. Deliver the final code repository and stakeholder presentation deck. |

---

## 📊 Dataset

**Name and Source:** WWEX Synthetic Multi-Carrier Shipping Dataset  
**Format:** CSV / TSV 
**Size:** Less than or equal to 1 GB (100% Google Colab Free Tier Compliant)  
**Location:** Provided directly by Challenge Advisor with data dictionary documentation 

### Key Details
- **Features Included:** Lane IDs, carrier codes, service levels, package weights and dimensions, seasonality indicators, weather features, and regional congestion metrics.
- **Targets Included:** Realized transit days (`transit_days`), delay status indicator, and carrier cost in USD (`carrier_cost_usd`).
- **Data Cleaning Task:** Intentionally contains real-world tabular noise (duplicate records, unit-error conversions, missing weights, whitespace noise, malformed ZIP codes) designed for student data preparation exercises.

---

## 🛠️ Suggested Approach

**ML Problem Type:** Tabular ML, Regression, Imbalanced Classification, Recommendation Systems.  
**Recommended Libraries:**
- `python` (Core language)
- `pandas`, `numpy` (Data manipulation, cleaning, and feature processing) 
- `scikit-learn` (Data splitting, metrics, linear/tree baselines, calibration)
- `xgboost` / `lightgbm` / `catboost` (Gradient-boosted decision trees for tabular performance)
- `matplotlib` / `seaborn` (EDA, error analysis, and fairness visualizations)

**Evaluation Metrics:**
- **Track A (Transit Days):** Mean Absolute Error (MAE in days, target < 0.84 MAE), RMSE, Share of predictions within $\pm 1$ day.
- **Track B (On-Time Risk):** PR-AUC (Precision-Recall Area Under Curve), ROC-AUC, Brier Calibration Score.
- **Track C (Recommendation):** Counterfactual cost/speed score under service-level constraints.

---

## 📚 Resources to Get Started

**Background Reading:**
- Supply Chain Logistics Basics: Understanding Parcel vs. LTL Freight Shipping Modes.
- Evaluation of Imbalanced Classification: Precision-Recall Curves vs. ROC Curves.

**Technical Tutorials:**
- Scikit-Learn Documentation: *Quantile Regression & Model Calibration Plots*.
- XGBoost / LightGBM Tutorials: *Handling Categorical Features and Custom Loss Functions*.
- Counterfactual Evaluation Concepts in Recommendation Systems.

---

## 🤝 How We'll Work Together

**Check-ins:** Weekly technical lab sections and biweekly Challenge Advisor meetings.  
**Communication:** BTT Project Slack Channel & GitHub Issue Tracker.  
**Response Time:** Within 24–48 hours for non-urgent technical inquiries.  
**Recommended Environment:**
- **Development Environment:** Google Colab (Free Tier CPU / T4 GPU).
- **Code Repository:** Public GitHub Repository with modular Python scripts (including `evaluate.py`).
- **Deliverables:** Runnable Google Colab Notebook, Cleaned Dataset Pipeline, Model Evaluation Report, and Final Stakeholder Presentation.

## 🚀 Getting Started

1. **Review this overview document** and note any questions for our first meeting
2. **Begin reviewing the dataset** using the link above
3. **Read the GitHub Projects documentation** [here](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)

I’m excited to work with you!

---

## ❓ Questions?

Please bring any questions to our first meeting during the week of August 24th (Break Through Tech’s Bridge to Studio - Session C). 
