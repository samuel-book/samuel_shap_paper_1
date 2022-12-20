# Abstract

*Objectives*: To understand between-hospital variation in thrombolysis use among patients in England And Wales who arrive at hospital within 4 hours of stroke onset.

*Design*: Machine learning was applied to the national stroke clinical audit data set, to learn which patients in each hospital would likely receive thrombolysis.

*Setting*: All hospitals providing emergency stroke care in England and Wales. Thrombolysis use in these patients ranged from 7\% to 49\% between hospitals.

*Participants*: 88,928 stroke patients who arrived at hospital within 4 hours of stroke onset, from 2016 to 2018.

*Intervention*: XGBoost machine learning base, coupled with a SHAP model for explainability.

*Main Outcome Measures*: SHAP values, providing estimates of how patient characteristics, and hospital identity, influence the odds of a patient receiving thrombolysis.

*Results*: The XGBoost/SHAP model revealed that the odds of receiving thrombolysis reduced 20 fold over the first 100 minutes of arrival-to-scan time, varied 30 fold depending on stroke severity, reduced 3 fold with imprecise onset time, fell 5 fold with increasing pre-stroke disability, and varied 15 fold between hospitals. The hospital identification (hospital SHAP value) explained 58% of the variance in between-hospital thrombolysis use. Compared with hospitals with higher thrombolysis use, hospitals with lower use were particularly less likely to give thrombolysis to patients with milder strokes, patients with prior disability, and patients with imprecise onset time.

*Conclusions*: The majority of the between-hospital variation in thrombolysis use in England and Wales may be explained by differences in hospital predisposition to use thrombolysis.
