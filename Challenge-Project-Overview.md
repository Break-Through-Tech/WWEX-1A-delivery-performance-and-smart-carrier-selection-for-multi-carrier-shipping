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

**Name and Source:** [TBD]   
**Format:** CSV / TSV   
**Size:** [TBD]   
**Location:** [To be provided directly by Challenge Advisor with data dictionary documentation]   

---

## 🛠️ Suggested Approach

**ML Problem Type:** Classification,Regression,Recommendation Systems 

**Recommended Libraries:**
- [e.g., pandas, scikit-learn, TensorFlow, Hugging Face]

**Evaluation Metrics:**
- [e.g., Accuracy, Precision/Recall, RMSE, BLEU score]

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
* [e.g., Your team's channel within Break Through Tech’s Discord space]
* [e.g., Email; please copy your teammates and AI Studio Coach]
* [e.g., Request a team check-in on Zoom]
* [Note: I will aim to respond within 48 hours. Please reach out to your AI Studio Coach with urgent questions.]

> 💡 **Challenge Advisor: Please update the above based on your availability and preference. If you are not able to answer questions or meet with fellows outside of the biweekly Lab Section check-ins, simply write in "N/A (only available during the official check-in times)"**

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
