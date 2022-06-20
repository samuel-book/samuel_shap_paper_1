# Introduction

*Applying explainable machine learning to national stroke audit data to explore variation in decisions to use thrombolysis*: Part of the NIHR *SAMueL* (Stroke Audit Machine Learning) project.

Kerry Pearn & Michael Allen

```{epigraph}
"Your decision to treat or not treat … That’s the difficult part. That’s the grey area where everyone does a different thing."

-- Stroke Consultant during interviews for SAMueL
```

## Background

Stroke is a common cause of adult disability. Expert opinion is that about one in five patients should receive clot-busting drugs to break up the blood clot that is causing their stroke, and this is the target set in the NHS long term plan. This clot-busting treatment is called thrombolysis. At the moment only about one in nine patients actually receive this treatment in the UK. There is a lot of variation between hospitals, which means that the same patient might receive different treatment in different hospitals.

In a previous project, [SAMueL-1](https://samuel-book.github.io/samuel-1/introduction/intro.html), we trained machine-learning models to predict whether any individual patient would receive thrombolysis in any hospital {numref}`Figure {number} <high_level_md>`. This allows us to investigate what differences in treatment are likely to be due to differences between patients, and what differences in treatment are likely to be due to differences between hospitals.

:::{figure-md} high_level_md
<img src="./images/ml_model_high_level.png" width="600">

A high level depiction of machine learning models trained to predict use of thrombolysis for any patient given 1) the hospital they attend, 2) patient and clinical information, and 3) pathway and process information. Machine learning models used are logistic regression, random forest, XGBoost, and neural networks.
:::

## Aims of this study

The aims of this study were 1) to apply *explainable machine learning* techniques to investigate the most significant features that drive decisions to use thrombolysis at different hospitals, and 2) to model and explain what are the the features that are most important in hospitals making *different* decisions about the same patient.

## Methods

In this study we used a machine learning method (XGBoost) to model decisions to give thrombolysis at each hopsital. Models were fitted to all hospital simultaneously, with hospital ID encoded as an input feature. We used Shapley values (using the `Shap` package) to explain model predictions at global and individual levels.

The XGBoost model described in this Jupyter Book used forward feature selection to choose the 8 features which led to the greatest accuracy (measured by ROC AUC). These features were:

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

### What are Shapley values?

> Shapley values are *'the average expected marginal contribution of one player after all possible combinations have been considered'*.

Or, imagine a pub quiz team with up to 3 people. Any number of people may actually turn up on the night:

* There are 8 possible combinations of players (including no-one turning up).

* The Shapley value for any team member describes the average difference in score when a particular player is present or absent compared to the average of all combinations of players.

The same principle may be applied in machine learning: How does any one feature (e.g. stroke severity, or age), on average, contribute to the prediction after considering all possible combinations of features? 

## Key findings

### Predicting thrombolysis use with an XGBoost model

The five most influential features in the XGBoost model predicting whether thrombolysis would be given or not were:

1. *Stroke type (infarction vs. haemorrhage)*: Use of thrombolysis depended on it being an infarction (clot).
2. *Time from arrival at hospital to time brain imaging was performed*: Predicted probability of using thrombolysis reduced with increasing time to scan.
3. *Stroke severity (NIHSS) on arrival*: Predicted probability of using thrombolysis was low at low NIHSS, rose with increasing NIHSS with a plateau at about NIHSS of 10-20, and then reduced with higher NIHSS.
4. *Stroke onset time type (precise vs. estimated)*: Predicted probability of using thrombolysis is increased with a precisely known  onset.
5. *Disability level (Rankin) before stroke*: Predicted probability of using thrombolysis reduced with increasing disability before stroke.

Shap plots may be used to explain predictions of any individual patient (e.g. {numref}`Figure {number} <waterfall_example>`). 

:::{figure-md} waterfall_example
<img src="./images/xgb_waterfall_low.png" width="800">

An example of a Shap *waterfall* plot showing the most influential features in influencing the model's prediction of a patient probability of receiving thrombolysis (in this case a patient with a very low probability of receiving thrombolysis). In this example the three most influential features, reducing the chance of receiving thrombolysis were 1) slow arrival-to-scan time (138 mins), low stroke severity (NIHSS 2), and 3 the hospital attended.
:::

### Predicting *differences* in thrombolysis use between hospitals with an XGBoost model

When an XGBoost model was trained to predict different choices in thrombolysis between units with a high or low propensity to use thrombolysis, the five most influential features were:

1. *Disability level (Rankin) before stroke*: lower thrombolysing units had a lower predicted probability of using thrombolysis with increasing disability before stroke.
2. *Stroke severity (NIHSS) on arrival*: lower thrombolysing units had a lower predicted probability of using thrombolysis with lower stroke severity.
3. *Stroke onset time type (precise vs. estimated)*: lower thrombolysing units had a lower predicted probability of using thrombolysis when the stroke onset time had been estimated.
4. *Time from onset to arrival at hospital*: lower thrombolysing units had a lower predicted probability of using thrombolysis with longer onset-to-arrival times.
5. *Time from arrival at hospital to time brain imaging was performed*: lower thrombolysing units had a lower predicted probability of using thrombolysis with longer arrival-to-scan times.

## Conclusions

*Explainable machine learning* techniques give significant insight into models prediction clinical decision-making. At a global level, Shap allows for an understanding of the relationship between feature values and the model prediction, and at an individual level Shap allows for an understanding of the most influential features in any single prediction.





