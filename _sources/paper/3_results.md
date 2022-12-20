# Results

## Variation in thrombolysis use

Thrombolysis use in the original data varied between hospitals, from 1.5% to 24.3% of all patients, and 7.3% to 49.7% of patients arriving within 4 hours of known stroke onset.

## Feature selection

25 features were selected by identifying one feature at a time that led to greatest improvement in Receiver Operating Characteristic (ROC) Area Under Curve (AUC). ROC AUC was measured using stratified 5-fold cross validation.

The best model with 1, 2, 5, 10, 25 & all features (60 features before one-hot encoding of fields) had ROC AUCs of 0.715, 0.792, 0.891, 0.919, 0.923 & 0.922. We selected 10 features for all subsequent work, which were:

* Arrival-to-scan time: Time from arrival at hospital to scan (mins)
* Infarction: Stroke type (1 = infarction, 0 = haemorrhage)
* Stroke severity: Stroke severity (NIHSS) on arrival
* Precise onset time: Onset time type (1 = precise, 0 = best estimate)
* Prior disability level: Disability level (modified Rankin Scale) before stroke
* Stroke team: Stroke team attended
* Use of AF anticoagulants: Use of atrial fibrillation anticoagulant (1 = Yes, 0 = No)
* Onset-to-arrival time: Time from onset of stroke to arrival at hospital (mins)
* Onset during sleep: Did stroke occur in sleep?
* Age: Age (as middle of 5 year age bands)

### Correlations within the 10 features

Correlations between the 10 features were measured using coefficients of determination (r-squared). All r-squared were less than 0.15, and all r-squared were less than 0.05 except 1) age and prior disability level (r-squared 0.146), and 2) onset during sleep and precise onset time (r-squared 0.078).

## Model accuracy

Model accuracy was measured using stratified 5-fold cross validation. The key results are shown below.

| measurement                      | mean  | std   |
|----------------------------------|-------|-------|
| actual positive rate             | 0.296 | 0.000 |
| actual negative rate             | 0.704 | 0.000 |
| predicted positive rate          | 0.294 | 0.002 |
| predicted negative rate          | 0.706 | 0.002 |
| accuracy                         | 0.850 | 0.004 |
| sensitivity (recall)             | 0.743 | 0.004 |
| specificity                      | 0.894 | 0.004 |
| precision                        | 0.747 | 0.007 |
| ROC AUC                          | 0.918 | 0.003 |
| Balanced sensitivity/specificity | 0.839 | 0.003 |

Overall accuracy was 85.0% (83.9% sensitivity and specificity could be achieved simultaneously). The ROC AUC was 0.918. The model predicted hospital thrombolysis use at each hospital with very good accuracy (r-squared = 0.977).

The appendix contains results for model validation of hospital thrombolysis curves, evaluation of variation in model prediction using bagging, learning rates, model calibration, and fine-tuning of model regularisation.

## An analysis of the characteristics of the most thrombolysable patient at each hospital

We identified the patient with the highest probability of thrombolysis (taken from 5-fold combined test set results) at each hospital, and compared the feature values of those patients to:

* All patients
* All patients who had received thrombolysis
* All patients who had not received thrombolysis

<img src="./images/02a_most_thrombolsyable_violin.jpg" width="600"/>

*Violin plots comparing feature values between the patient with the highest probability of thrombolysis at each hospital with all patients, all patients who had received thrombolysis, and all patients who had not received thrombolysis. Horizontal lines show the mean value of each group.*

Compared with the other groups, the most thrombolysable patients ....

* Had shorter arrrival-to-scan times
* Had an infarction stroke type
* Had stroke servities with NIHR 5-25
* Had a precise onset time
* Had lower pre-stroke disability
* Were not taking anticoagulant medication
* Had shorter onset-to-arrival times
* Did not have onset during sleep
* Were younger

## Explaining model predictions with SHAP

### Waterfall plots for individual patient predictions

Waterfall plots show the influence of features for an individual prediction. We generally handle SHAP values as how they affect log odds of receiving thrombolysis, but for individual predictions, probability plots are more intuitive. The examples below are for a patient with low (top) and high (bottom) probability of receiving thrombolysis. The model starts with a base prediction of a 24% probability of receiving thrombolysis, before feature values are taken into account. For the patient with a low probability of receiving thrombolysis, the two most influential features reducing the probability of receiving thrombolysis are a long arrival-to-scan time (138 minutes) and a low stroke severity (NIHSS=2). For the patient with a high probability of receiving thrombolysis, the two most influential features increading the probability of receiving thrombolysis are a short arrival-to-scan time (17 minutes) and a moderate stroke severity (NIHSS=14). 

<img src="./images/waterfall.jpg" width="500"/>

*Waterfall plots showing the influence of each feature on the predicted probability of a single patient receiving thrombolysis. Top: An example of a patient with a low probability (2.6%) of receiving thrombolysis. Bottom: An example of a patient with a high probability (95.7%) of receiving thrombolysis.*

### The relationship between feature values and the odds of receiving thrombolysis

SHAP values provide the influence of each feature, as the change in log-odds of receiving thrombolysis. SHAP values expressed as log-odds are additive.

Violin plots show the relationship between feature values and SHAP values for individual patients (the bar in each violin shows the median value). Key observations are:

* Stroke type: As expected,  the SHAP values for stroke types effectively eliminated any chance of receiving thrombolysis for non-ischaemic (haemorrhagic) stroke.

* Arrival-to-scan time: The odds of receiving thrombolysis reduced by about 20 fold over the first 100 minutes of arrival to scan time.

* Stroke severity (NIHSS): The odds of receiving thrombolysis was lowest at NIHSS 0, rose and peakws at NIHSS 15-25, and then fell again with higher stroke severity. The difference between minimum odds (at NIHSS 0) and maximum odds (at 15-25) of receiving thrombolysis was 30-35 fold.

* Stroke onset time type (precise vs. estimated): The odds of receiving thrombolysis were about 3 fold greater for precise onset time than estimated onset time.

* Disability level (Rankin) before stroke. The odds of receiving thrombolysis fell about 5 fold between mRS 0 and 5.

<img src="../xgb_10_features/output/03_xgb_10_features_thrombolysis_shap_violin.jpg" width="800"/>

*Violin plots showing the relationship between SHAP values and feature values. The horizontal line shows the median SHAP value. SHAP values were taken from the training set of the first of 5 k-fold train/test splits.*

### Hospital SHAP values

When examining SHAP, we took the hospital SHAP values for patients attending each hospital. If we examined the hospital SHAP for all patients, it would be dominated by those not-attending each hospital (i.e. coded zero in the one-hot encoding). When we examined the hospital SHAP values only for patients attending each hospital, the hospital SHAP values ranged from -1.4 to +1.4. This range of SHAP (log odds) represents a 15 fold difference in odds of receiving thrombolysis (most are in the range of -1 to +1, but this still represents a 7-8 fold difference in odds of receiving thrombolysis).

We compared the hospital SHAP value with the observed thrombolysis use at each hospital. Hospital SHAP correlated with observed thrombolysis rate with an r-squared of 0.582, suggesting that 58% (P<0.0001) of the between-hospital variance in thrombolysis use may be explained by the hospitals' SHAP values, that is the hospitals' predisposition to use thrombolysis.

When using the 10k cohort, changing the one-hot encoding to mimic all patients attending each of the 132 hospitals, we found that the median hospital SHAP value for the 10k patients correlated very closely with the predicted thrombolysis use in the 10k cohort at each hospital (r-squared = 0.947), confirming that the hospital SHAP is providing a key insight into a hospital's propensity to use thrombolysis.

<img src="./images/99_twin_correlation_scatter.jpg" width="800"/>

*Correlation between median hospital SHAP value and thrombolysis use at each hospital. Left: Observed thrombolysis use at each hospital. Right: Predicted thrombolysis use if the same 10k cohort of patients attended each hospital.*

### SHAP interactions

SHAP values are composed of a *main effect* of a feature, and *interaction* with other features. The interaction may either strengthen or attenuate the main effect. For example the main effect of knowing a stroke onset time precisely is to increase the odds of receiving thrombolysis, as indicated by a positive SHAP values. Individual hospitals may selectively adjust the influence of knowing the stroke onset time precisely, with some teams strengthening the effect, leading to a larger difference in odds of receiving thrombolysis depending on whether stroke onset time is known precisely, whereas other teams will attenuate the main effect, leading to a smaller difference in odds of receiving thrombolysis.

<img src="./images/12aa_three_way_shap_adjustment.jpg" width="800"/>

*Adjustment of SHAP main effects by individual hospital. Each row first (left) shows the main SHAP effect of the feature, then (middle) a hospital where the SHAP interaction attenuates the main effect, and finally (right) a hospital where the SHAP interaction strengthens the main effect. Top row (red): main SHAP effect and adjusted SHAP values for precise stroke onset time. Middle row (green): main SHAP effect and adjusted SHAP values for stroke severity. Bottom row (blue): main SHAP effect and adjusted SHAP values for pre-stroke disability.*

## Subgroup analysis

We analysed the observed and predicted use of thrombolysis in subgroups of patients.Those groups are:

  * Mild stroke severity (NIHSS < 5)
  * No precise onset time
  * Existing pre-stroke disability (mRS > 2)
  * An *'ideal'* thrombolysable patient:
    * Stroke severity NIHSS in range 10-25
    * Arrival-to-scan time < 30 minutes
    * Stroke type = infarction
    * Precise onset time = True
    * Prior disability level (mRS) = 0
    * No use of AF anticoagulants
    * Onset-to-arrival time < 90 minutes
    * Age < 80 years
    * Onset during sleep = False
    
For the observed thrombolysis use, data was limited to the patients attending each hospital. For the predicted thrombolysis use the predictions were based on the 10k patient cohort for all hospitals.

The figure below shows a boxplot of either observed and predicted use of thrombolysis, broken down by subgroup.

<img src="../xgb_10_features/output/15a_actual_vs_modelled_subgroup_violin.jpg" width="600"/>

*Boxplot for either observed (top) or predicted (bottom) use of thrombolysis for subgroups of patients.*

The three subgroups of NIHSS <5, no precise stroke onset time, and pre-stroke mRS > 2, all had reduced thrombolysis use, and combining these non-ideal features reduced thrombolysis use further.

The observed and predicted thrombolysis use show the same general patterns, but some smalled differences existed:

* The use of thrombolysis in *ideal* patients is a little low in the observed vs actual results (mean hospital thrombolysis use = 89% vs 99%).
* The predicted results show a stronger effect of combining non-ideal features.
* The observed thrombolysis rate shows higher between-hospital variation than the predicted thrombolysis rate. This may be partly explained by the observed thrombolysis rate being on different patients at each hospital, but may also be partly explained by actual use of thrombolysis being slightly more variable than predicted thrombolysis use (which will follow general hospital patterns, and will not include, for example, between-clinician variation at each hospital).

When we looked at thrombolysis use in subgroups at each hospital we see that the observed thrombolysis use tended to reduce in parallel, along with total thrombolysis use, suggesting a shared caution in use of thrombolysis in 'less ideal' patients. 