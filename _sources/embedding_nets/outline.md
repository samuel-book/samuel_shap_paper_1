# Outline

## Embedding neural networks

*Embedding* converts a categorical variable into a projection onto n-dimensional space [1], and has been shown to be an effective way to train neural network when using categorical data, while also allowing a measure of similarity/distance between different values of the categorical data, Here we use embedding for hospital ID. We also convert patient data and pathway data into an embedded vector (this may also be known as *encoding* the data in a vector with fewer dimensions than the original data set for those groups of features).

[1] Guo C, & Berkhahn F. (2016) Entity Embeddings of Categorical Variables. arXiv:160406737 [cs] http://arxiv.org/abs/1604.06737


## Basic methodology

The model contains three subnets that take portions of the data. The output of these subnets is an n-dimensional vector. In this case the output is a 1D vector, that is each subnet is reduced to a single value output. The subnets created are for:

1. *Patient clinical data*: Age, gender, ethnicity, disability before stroke, stroke scale data. Pass through one hidden layer (with 2x neurons as input features) and then to single neuron with sigmoid activation.

2. *Pathway process data*: Times of arrival and scan, time of day, day of week. Pass through one hidden layer (with 2x neurons as input features) and then to single neuron with sigmoid activation.

3. *Hospital ID* (one-hot encoded): Connect input directly to single neuron with sigmoid activation.

The outputs of the three subnet outputs are then passed to a single neuron with sigmoid activation for final output. The outputs of the subnets may also be accessed separately. A schematic of the embeddign neural network design is shown in {numref}`Figure {number} <subnet_diagram>`

:::{figure-md} subnet_diagram
<img src="./images/embedding_1d_with_subnet_output.png" width="800">

An overview of the embedding neural network.
:::

## Notebooks in this section

> All the notebooks in this section us 1D embedding; that is each of the three subnets have a single number output.

* *Train and save k-fold networks*:
    * Train 5 neural networks based on stratified k-fold train/test data splits.

* *Assess accuracy of k-fold networks*:
    * Measure a range of accuracy scores (e.g. accuracy, sensitivity, specificity, F1, etc).
    * Plot Receiver Operator Characteristic Curve and measure AUC.
    * Identify cross-over point of sensitivity and specificity.
    * Save individual patient predictions.
    * Compare predicted and observed thrombolysis use at each hospital.
    * Check model calibration.
    
* *Train and save network for 10K patient subset*:
    * To train and save a 1D embedding network for a 10K patient subset.

* *Investigating the output of neural net embedding subnets*:
    * To assess predicted thrombolysis in a 10K cohort of patients if that same cohort attended each of 132 hospitals.
    * To investigate the output of the hospital and clinical embedding subnets of the embedding neural network.
    * To examine the link between hospital embedding subnet output and use of thrombolysis in hospitals - both the actual thrombolysis use, and the predicted thrombolysis use of a 10k set of patients passed through all hospital models.
    * To examine the link between the patient clinical feature embedding subnet output and the use of thrombolysis, and the link between patient features and the clinical feature embedding subnet output.

* *Compare local thrombolysis decisions with benchmark* decisions:
    * Benchmark decisions are decisions made my the majority of the top 30 hospitals as judged by their expected thrombolysis use in a standard 10K cohort of patients.
    * Get predicted thrombolysis decisions for all patients at the 30 benchmark hospitals.
    * Check similarity between local decisions and benchmark decisions.
    * Estimate thrombolysis use at each hospital if benchmark decisions made.
    * Save comparison of local and benchmark decisions.

* *Collate embedding subnet outputs*:
    * Collate and save embedding subnet output for all patients (scale outputs 0-1 so that 1 = highest probability of receiving thrombolysis). 

* *Understanding influence of features of neural net subnet outputs*:
    * Fit an XGBoost model to predict thrombolysis use given three embedding subnet outputs. Assess accuracy and ROC AUC.
    * Get model *importances* and Shap values for this simple XGBoost model. Show relationships between Shap values and model output.
    * Fit XGBoost regressors to predict clinical and pathway subnet output, get Shap values for the regressor, and show relationships between Shap values and model output.
    
    
    
    
