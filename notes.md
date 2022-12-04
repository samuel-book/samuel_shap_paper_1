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

* Stroke type - there is a difference of about 8 in log odds of receiving thrombolysis between infarction and haemorrhagic stroke. As expected this, effectively eliminates any chance of receiving thrombolysis for haemorrhagic stroke.

* Arrival-to-scan time: The odds of receiving thrombolysis reduces by about 20 fold over the first 100 minutes (SHAP log odds difference ~3).

* Stroke severity (NIHSS): The odds of receiving thrombolysis peaks at NIHSS 15-25. The difference between minimum (at NIHSS 0) and maximum (at 15-25) odds of receiving thrombolysis is 30-35 (SHAP log odds difference ~3.5)

* Stroke onset time type (precise vs. estimated): The odds of receiving thrombolysis are about 3 fold greater for precise onset time than estimated onset time (SHAP log odds difference ~1.2). 

* Disability level (Rankin) before stroke. The odds of receiving thrombolysis falls about 5 fold between mRS 0 and 5 (SHAP log odds difference ~1.7).

* The hospital SHAP values ranges from -1.4 to +1.4. This range of SHAP (log odds) represents a 15 fold difference in odds of receiving thrombolysis (most are in the range of -1 to +1, but this still represents a 7-8 fold difference in odds of receiving thrombolysis). 

SHAP values were consistent across k-fold splits

Contains examples of waterfall plots (odds and probabilities)
Scatter plots and violin plots
SHAP values for hospitals for patients attending each hospital


## Notebook 03a: Isolation of hospital 

* Hospital SHAP values also contain all zero values of one-hot encoded hospital attendence, so there is a need to isolate the patients attending each hospital to calulate a hospital SHAP.

## Notebook 03b: Isolation of hospital, but main SHAP effect only

## Notebook 03c: General notebook on hospital SHAP

* 58% of the variability in hospital thrombolysis rate can be explained by the SHAP value for the one-hot encoded hospital feature (the median of those instances that attend the hospital); 56% of the variance may be explained by the SHAP main effect.

Across all the hopsitals SHAP values and SHAP main effect have approx the same median and quartiles, but the full SHAP (with interactions) have a larger range. For each hospital the full SHAP has significantly larger range (inc IQR_ than the main effect).

## Notebook 04 - 10k thrombolysis and SHAP

* Predict thrombolysis rate for 10k cohort across hospitals
* Thrombolysis rates vary from 10% to 45% across hospitals
* 10% of the variance in 10k thrombolysis rate can be explained by hospital size
* 10k thrombolysis rate correlates closely with hospital SHAP value: 92% of the variance in 10k thrombolysis rate is explained by hospital SHAP
* There is more agreement on who should not receive thrombolysis than who should receive thrombolysis: 87.7% of patients have 80% of hospitals agree on treatment. For those patients that did actually receive thrombolysis, 79.0% of patients have 80% of hospitals agree to thrombolyse. For those patients that did not actually receive thrombolysis, 91.5% of patients have 80% of hospitals agree not to thrombolyse. 

## Notebook 05  - compare local and benchmark decisions

* 83.3% decisions are identical between local and benchmark decisions.
* Thrombolysis use would be increased 20.7% if benchmark decisions were made at all hospitals.
* The ratio of benchmark:local thrombolysis use was 0.7 to 2.1.

## Notebook 06 - Predicting differences between local and benchmark decisions

* We can predict those that will receive thrombolysis at a local unit, out of those who will be thrombolysed by the majority of the benchmark hospitals, with 67% accuracy (AUC 0.72).

* The five most important distinguishing features are:
  * Stroke severity
  * Prior disability
  * Precise onset time   * Arrival to scan time
  * Onset to arrival time






