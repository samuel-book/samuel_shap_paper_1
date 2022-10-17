# Outline of notebooks

Experiments using an XGBoost classifier are performed after first reducing the number of features using feature selection. The features selected were:

* S2BrainImagingTime_min
* S2StrokeType_Infarction
* S2NihssArrival
* S1OnsetTimeType_Precise
* S2RankinBeforeStroke
* StrokeTeam
* AFAnticoagulent_Yes
* S1OnsetToArrival_min

Note: The [GitHub repository](https://github.com/samuel-book/samuel_shap_paper_1) also includes XG-Boost models using all available features.

## XGBoost models

XGBoost is a decision tree-based method for prediction.

We use *Shapley values* (or SHAP values, which is a particular method of estimating Shapley values) to provide an estimate of how much any particular feature influences the model decision. When Shapley values are averaged they provide a measure of the overall influence of a feature.

Shapley values may be used across model types, and so provide a model-agnostic measure of a feature’s influence. This means that the influence of features may be compared across model types, and it allows black box models like neural networks to be explained, at least in part.

For more on Shapley values in general see Chris Molner’s excellent book chapter:

https://christophm.github.io/interpretable-ml-book/shapley.html

More information on the `shap` library, used to estimate Shapley values, may be found at: https://shap.readthedocs.io/en/latest/index.html

## Notebooks in this section

*Glossary of technical terms*:
    * A glossary of technical terms used throughout this online book.

### General examples of SHAP

* *Probability, odds, and SHAP values (log odds shifts)*:
    * Comparing probabilities and odds.
    * Showing how SHAP shows us how much odds and probabilities change

* *A simple worked example of Shapley values*:
    * A very simple example of Shapley values based on three people who may contribute to the scores of a pub quiz team.
    * A quick demonstration of the SHAP library to estimate Shapley values.
    
* *Describing model predictions, using SHAP values and SHAP interactions: Using Titanic survival as an example use case*
	* Demonstrating SHAP and SAHP interaction values with a four-feature model of survival on board the Titanic.

### Predicting thrombolysis use and explaining the predictions with SHAP

* *XGBoost feature selection*:
    * Select up to 25 features using forward feature selection. Features are selected sequentially, choosing the feature that leads to most improvement in area under the ROC curve (ROC AUC) score. Eight were chosen.

* *Check correlation between selected features*:
    * Check correlation between 8 features selected by feature selection.
    
* *Assess accuracy of k-fold models*:
    * Measure a range of accuracy scores (e.g. accuracy, sensitivity, specificity, F1, etc).
    * Plot receiver operator characteristic (ROC) curve and measure ROC AUC.
    * Identify cross-over point of sensitivity and specificity.
    * Save individual patient predictions.
    * Compare predicted and observed thrombolysis use at each hospital.
    * Check model calibration.
    
* *Explaining XGBoost model predictions with Shapley values*:
    * Fit XGBoost model to k-fold train/test splits.
    * Get SHAP values for each k-fold split.
    * Examine consistency of SHAP values across k-fold splits.
    * Examine consistency of XGBoost Importance across k-fold splits.
    * Compare SHAP values and XGBoost Importance.
    * Further analyse SHAP values with:
        * Beeswarm plots
        * Waterfall plots
        * Scatter plots
    * Show example of SHAP waterfall plot as *probability* rather than *log-odds ratio*.
    
### Comparing thrombolysis decisions between hospitals
  
* *A comparison of 10k cohort thrombolysis rates across hospitals, and comparison with hospital SHAP values*:
    * Train XGBoost model on all data except for a 10K set of patients.
    * Predict use of thrombolysis in 10K cohort at each of 132 hospitals (by changing hospital one-hot encoding).
    * Compare mean hospital SHAP for each hopsital with the predicted 10K cohort thrombolysis rate for each hospital.

* *Compare local thrombolysis decisions with benchmark decisions*:
    * Benchmark decisions are decisions made by the majority of the top 30 thrombolysing hospitals as judged by their expected thrombolysis use in a standard 10K cohort of patients.
    * Get predicted thrombolysis decisions for all patients at the 30 benchmark hospitals.
    * Check similarity between local decisions and benchmark decisions.
    * Estimate thrombolysis use at each hospital if benchmark decisions made.
    * Save comparison of local and benchmark decisions.
    
* *Predicting differences between local and benchmark decisions*:
    * We build a model to predict those patients, out of patients who would be thrombolysed by the majority of the benchmark hospitals, who would *not* be thrombolysed at the bottom 30 thrombolysing hospitals, as assessed by the predicted thrombolysis use in the 10k cohort set.. 
    * Of all those patients thrombolysed by benchmark decision, build an XGBoost model to predict which patients would *not* be thrombolysed at a local unit.
    * Get SHAP values for predictions.
        * Check consistency of SHAP values and XGBoost Importance across k-fold replications
        * Compare SHAP values and XGBoost Importance
        * Analyse relationship between features and SHAP values with beeswarm, waterfall, and scatter plots.
        
* *Comparing SHAP values between high and low thrombolysing hospitals*:
    * Plot SHAP values for the top and bottom 30 hospitals, as assessed by the predicted thrombolysis use in the 10k cohort set.
    
* *Plotting thrombolysis rate by feature value for low and high thrombolysing hospitals*:
    * Plot the relationships between feature values and thrombolysis use for the top and bottom 30 thrombolysing hospitals, as assessed by the predicted thrombolysis use in the 10k cohort set.
