# Notes on notebook key content

## Notebook 01: feature selection

* Feature selection
* All features ROC=0.922
* 10 features ROC 0.919

10 features are:
* Arrival-to-scan time: Time from arrival at hospital to scan (mins)
* Infarction: Stroke type (1 = infarction, 0 = haemorrhage)
* Stroke severity: Stroke severity (NIHSS) on arrival
* Precise onset time: Onset time type (1 = precise, 0 = best estimate)
* Prior disability level: Disability level (modified Rankin Scale) before stroke
* Stroke team: Stroke team attended
* Use of AF anticoagulents: Use of atrial fibrillation anticoagulant (1 = Yes, 0 = No)
* Onset-to-arrival time: Time from onset of stroke to arrival at hospital (mins)
* Onset during sleep: Did stroke occur in sleep?
* Age: Age (as middle of 5 year age bands)

## Notebook 02: Accuracy

* Overall accuracy = 84.8%
* Using nominal threshold (50% probability), specificity (89.4%) is greater than sensitivity (74.0%)
* The model can achieve 83.7% sensitivity and specificity simultaneously
* ROC AUC = 0.918
* The model is well calibrated

## Notebook 03: General SHAP

The five most influential features as judged by SHAP were:

* Stroke type - 8 fold difference in log odds effectively eliminates any chance of thrombolysis.
* Arrival-to-scan time - log odds reduces by about 3 over the first 100 minutes (a 20 fold reduction in odds)
* Stroke severity (NIHSS) - peaks at NIHSS 15-25. Difference between min and max about 3.5 log odds = 30-35 fold difference in odds.
* Stroke onset time type (precise vs. estimated) - 1.2 difference in log odds, about 3 fold difference
* Disability level (Rankin) before stroke. About 1.7 log odds difference between mRS 0 and 5, a 5 fold difference in odds

SHAP values were consistent across k-fold splits

Contains examples of waterfall plots (odds and probabilities)
Scatter plots and violin plots
SHAP values for hospitals for patients attending each hospital

Range of hospital SHAP -1.4 to +1.4. This range of SHAP (log odds) represents a 15 fold difference in odds of receiving thrombolysis (most are in the range of -1 to +1, but this still represents a 7-8 fold difference in odds of receiving thrombolysis). Standard Deviation is 0.52 (representing a 2.8 fold difference in odds with +/- 1SD).

## Notebook 03a: Isolation of hospital 

* Hospital SHAP values also contain all zero values of one-hot encoded hospital attendence, so there is a need to isolate the patients attending each hopsital to calulate a hospital SHAP.

* 58% of the variability in hospital thrombolysis rate can be explained by the SHAP value for the one-hot encoded hospital feature (the median of those instances that attend the hospital).


