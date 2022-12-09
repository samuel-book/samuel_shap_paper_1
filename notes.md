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

Across all the hospitals SHAP values and SHAP main effect have approx the same median and quartiles, but the full SHAP (with interactions) have a larger range. For each hospital the full SHAP has significantly larger range (inc IQR_ than the main effect).

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

## Notebook 07 - correlation between 10 features
 * There are only very weak correlations between the selected features with no R-squared being greater than 0.15, and all but two being lower than 0.05.

 ## Notebook 8 - compare SHAP on top/bottom 30 or 9 hospitals

 * Low and high thrombolysing hosptials have the same general pattern: that a patient is more likely to recieve thromboylsis if they have a mid-level stroke severity. This is overlaid over different hospital SHAP values.
* There is an average diffeence in hospital SHAP of 1.31 between the top and bottom 30 hospitals, and 1.98 between the top and bottom 9 hospitals. These differences are equivalent to a difference in odds of receiving thrombolysis of 3.7 and 7.2 respectiviely.
* Lower thrombolysing hospitals have a more exagerrated effect of stroke severity, though other features such as disability before stroke, use of AF anticoagulants and arrival to scan time look similar.

## Notebook 09 - Plotting thrombolysis rate by feature value for low and high thrombolysing hopsitals

* Thrombolysis use in low thrombolysing hospitals follows the same general relationship with feature values as the high thrombolysing hospitals, but thrombolysis is consistently lower.

## Notebook 12a - Describing model predictions, using SHAP values and SHAP interactions

* The proportions of SHAP values comings from the main effect and the interactions are 62% and 8% respectively.
* Viewing them as a grid of SHAP dependency plots clearly shows the overall relationships that the model uses to derive it's predictions for the whole dataset.
* Some key interactions are:
  * If a stroke is haemorrhagic, then the interaction is such that the presence of haemorrhage cancels out other SHAP effects (i.e. stroke severity does not matter if the stroke is haemorrhagic).
  * Likewise elsewhere we find that features that each give negative SHAP values alone have an interaction that attenuates the combines effect a little.

## Notebook 12aa - Describing model predictions, using SHAP values and SHAP interactions focusing on how Hospitals interact with Precise Onset Time

* The main effect of the *precise onset time* is that if onset time is known precicely then SHAP (log odds) is increased by 0.42, otherwise it is reduced by 0.85.
* The interaction between the hopsital and precise onset time then adjusts this main effect. Some hopsitals strengthen the effect, and other hopsitals atenuate the effect.

## Notebook 12ab - Describing model predictions, using SHAP values and SHAP interactions focusing on how Hospitals interact with Stroke Severity

* The main effect of stroke severity is to signficantly reduce the odds of receiving thrombolysis for mild strokes (NIHSS 0-5), increase the odds of receiving thrombolysis for more moderate to sever strokes (NIHSS 6-32), and then reduce the odds of receiving thrombolysis for very severe stroke strokes (NIHSS 33+).
* The interaction between the hopsital and stroke severity then adjusts this main effect. Some hopsitals strengthen the effect, and other hopsitals atenuate the effect.

## Notebook 12ac - Describing model predictions, using SHAP values and SHAP interactions focusing on how Hospitals interact with pre-stroke disability

* The main effect of prior disability is to progressively reduce the odds of receiving thrombolysis with increasing disability (a SHAP of +0.3 for mrS=0 down to -1.50 for mRS=5).
* The interaction between the hopsital and stroke severity time then adjusts this main effect. Some hopsitals strengthen the effect, and other hopsitals atenuate the effect.

## Notebook 20 - Investigating the effect of patient features by varying feature values in artificial patients

The number of hospitals giving thrombolysis:

* Base patient: 132 (99%)
* Base patient, but NIHSS = 5: 123 (93%)
* Base patient, but pre-stroke disability = 2: 125 (95%)
* Base patient, but estimated stroke onset time: 109 (83%)

Combining two marginal features:

* Base patient, but NIHSS = 5 and pre-stroke disability = 2: 78 (59%)
* Base patient, but NIHSS = 5 and estimated stroke onset time: 30 (23%)
* Base patient, but pre-stroke disability = 2 and estimated stroke onset time: 26 (20%)

Combining three marginal features:

* Base patient, but NIHSS = 5, pre-stroke disability = 2, estimated stroke onset time: 2 (1.5%)

## Measuring model variation with bagging

Here we evaluate the variation in model predictions (at a patient level) and predicted 10k thrombolysis rate using bootstrap models.

We apply two bootstrap methods, In the traditional method we fit multiple models to bootstrapped samples of the training set (random sampling with replacement).

In the Bayesian bootstrap method we fit multiple models, with the training set weighted each time by sampling from a Dirichlet distribution (see https://towardsdatascience.com/the-bayesian-bootstrap-6ca4a1d45148).

These methods give similar results - the average standard deviation in patient-level prediction (the probability of a patient receiving thrombolysis) is about 0.05, but this ranges from about 0.01 to 0.13 dependening on the predicted probability (with greatest variance around 50% predicted probability of receiving thrombolysis). The average standard deviation in predicting the hospital's expected thrombolysis rate is 0.015-0.02. 

## Evaluation of XGBoost learning rates on distribution of 10k thrombolysis rates

A learning rate of 0.5 is selected to prevent over-regularisation of hopsital one-hot encoding
