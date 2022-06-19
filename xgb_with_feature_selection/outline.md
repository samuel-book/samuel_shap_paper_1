# Outline

Experiments using an XGBoost classifier are performed after first reducing the number of features using feature selection. The features selected were:

* S2BrainImagingTime_min
* S2StrokeType_Infarction
* S2NihssArrival
* S1OnsetTimeType_Precise
* S2RankinBeforeStroke
* StrokeTeam
* AFAnticoagulent_Yes
* S1OnsetToArrival_min

Note: The GitHub repository also includes the same notebooks, but for XGBoost models using all available features:

https://github.com/samuel-book/samuel_shap_paper_1


## XGBoost models

XGBoost are a decision tree-based method for prediction.

We use *Shapley values* to provide an estimate of how much any particular feature influences the model decision. When Shapley values are averaged they provide a measure of the overall influence of a feature.

Shapley values may be used across model types, and so provide a model-agnostic measure of a feature’s influence. This means that the influence of features may be compared across model types, and it allows black box models like neural networks to be explained, at least in part.

For more on Shapley values in general see Chris Molner’s excellent book chapter:

https://christophm.github.io/interpretable-ml-book/shapley.html

More information on the `shap` library, used to estimate Shapley values, may be found at: https://shap.readthedocs.io/en/latest/index.html

## Notebooks in this section

* *XGBoost feature selection*:
    * Select up to 25 features using forward feature selection. Features are selected sequentially, choosing the feature that leads to most improvement in ROC AUC score (8 were chosen).

* *Assess accuracy of k-fold models*:
    * Measure a range of accuracy scores (e.g. accuracy, sensitivity, specificity, F1, etc).
    * Plot Receiver Operator Characteristic Curve and measure AUC.
    * Identify cross-over point of sensitivity and specificity.
    * Save individual patient predictions.
    * Compare predicted and observed thrombolysis use at each hospital.
    * Check model calibration.
    

* *Explaining XGBoost model predictions with Shapley values*:
    * Fit XGBoost model to k-fold train/test splits.
    * Get Shap values for each k-fold split.
    * Examine consistency of Shap values across k-fold splits.
    * Examine consistency of XGBoost Importance across k-fold splits.
    * Compare Shap values and XGBoost Importance.
    * Further analyse Shap values with:
        * Beeswarm plots
        * Waterfall plots
        * Scatter plots
    * Show example of Shap waterfall plot as *probability* rather than *log odds ratio*.
  
* *A comparison of 10K cohort thrombolysis rates across hospitals*:
    * Train XGBoost model on all data except for a 10K set of patients
    * Predict use of thrombolysis in 10K cohort at each of 132 hospitals (by changing hospital one-hot coding).

* *Compare local thrombolysis decisions with benchmark decisions*:
    * Benchmark decisions are decisions made my the majority of the top 30 hospitals as judged by their expected thrombolysis use in a standard 10K cohort of patients.
    * Get predicted thrombolysis decisions for all patients at the 30 benchmark hospitals.
    * Check similarity between local decisions and benchmark decisions.
    * Estimate thrombolysis use at each hospital if benchmark decisions made.
    * Save comparison of local and benchmark decisions.
    
* *Predicting differences between local and benchmark decisions*:
    * This experiment focuses on hospitals who would give thrombolysis to at least 50% more patients if the majority vote of 30 benchmark hospitals were applied. We build a model to predict those patients, out of patients who will be thrombolysed by the majority of the benchmark hospitals, who will thrombolysed at a local unit. 
    * Of all those patients thrombolysed by benchmark decision, build an XGBoost model to predict which patients, would be thrombolysed at a local unit.
    * Get Shap values for predictions.
        * Check consistency of Shap values and XGBoost Importance across k-fold replications
        * Compare Shap values and XGBoost Importance
        * Anlyse relationship between features and Shap values with beeswarm, waterfall, and scatter plots.
        
