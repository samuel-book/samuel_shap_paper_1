# Applying explainable machine learning to national stroke audit data to explore variation in decisions to use thrombolysis

Kerry Pearn & Michael Allen

```{epigraph}
"Your decision to treat or not treat … That’s the difficult part. That’s the grey area where everyone does a different thing."

-- Stroke Consultant during interviews for SAMueL
```

## Background

Stroke is a common cause of adult disability. Expert opinion is that about one in five patients should receive clot-busting drugs to break up the blood clot that is causing their stroke, and this is the target set in the NHS long term plan. This clot-busting treatment is called thrombolysis. At the moment only about one in nine patients actually receive this treatment in the UK. There is a lot of variation between hospitals, which means that the same patient might receive different treatment in different hospitals.

In a previous project, [SAMueL-1](https://samuel-book.github.io/samuel-1/introduction/intro.html), we trained machine-learning models to predict whether any individual patient would receive thrombolysis in any hospital {numref}`Figure {number} <high_level_md>`.

:::{figure-md} high_level_md
<img src="./images/ml_model_high_level.png" width="600">

A high level depiction of machine learning models trained to predict use of thrombolysis for any patient given 1) the hospital they attend, 2) patient and clinical information, and 3) pathway and process information. Machine learning models used are logistic regression, random forest, XGBoost, and neural networks.
:::

## Aims of this study

The aims of this study were 1) to apply *explainable machine learning* techniques to investigate the most significant features that drive decisions to use thrombolysis at different hospitals, and 2) to model and explain what are the the features that are most important in hospitals making *different* decisions about the same patient.

## Methods

In this study we used with XGBoost or embedding neural networks to model decisions to give thrombolysis. Models were fitted to all hospital simultaneously, with hospital ID encoded as a *one-hot* vector. We used Shapley values (using the `Shap` package) to explain model predictions at global and individual levels. 

## Key findings

The five most influential features in the XGBoost model predicting whether thrombolysis would be given or not were:

1. Stroke type (infarction vs. haemorrhage): Use of thrombolysis depended on it being an infarction (clot).
2. Time from arrival at hospital to time brain imaging was performed: Predicted probability of using thrombolysis reduced with increasing time to scan.
3. Stroke severity (NIHSS) on arrival: Predicted probability of using thrombolysis was low at low NIHSS, rose with increasing NIHSS with a plateau at about NIHSS of 10-20, and then reduced with higher NIHSS.
4. Stroke onset time type (precise vs. estimated): Predicted probability of using thrombolysis is increased with a precisely known  onset.
5. Disability level (Rankin) before stroke: Predicted probability of using thrombolysis reduced with increasing disability before stroke.

When an XGBoost model was trained to predict different choices in thrombolysis between units with a high or low propensity to use thrombolysis, the five most influential features were:

1. Disability level (Rankin) before stroke: lower thrombolysing units had a lower predicted probability of using thrombolysis with increasing disability before stroke.
2. Stroke severity (NIHSS) on arrival: lower thrombolysing units had a lower predicted probability of using thrombolysis with lower stroke severity.
3. Stroke onset time type (precise vs. estimated): lower thrombolysing units had a lower predicted probability of using thrombolysis when the stroke onset time had been estimated.
4. Time from onset to arrival at hospital: lower thrombolysing units had a lower predicted probability of using thrombolysis with longer onset-to-arrival times.
5. Time from arrival at hospital to time brain imaging was performed: lower thrombolysing units had a lower predicted probability of using thrombolysis with longer arrival-to-scan times.

Shap plots may be used to explain predictions of any individual patient (e.g. {numref}`Figure {number} <waterfall_example>`). 

:::{figure-md} waterfall_example
<img src="./images/xgb_waterfall_low_probability.jpg" width="600">

An example of a Shap *waterfall* plot showing the most influential features in influencing the model's prediction of a patient receiving thrombolysis (in this case a patient with a very low probability of receiving thrombolysis).
:::

## Conclusions

*Explainable machine learning* techniques give significant insight into models prediction clinical decision-making. At a global level, Shap allows for an understanding of the relationship between feature values and the model prediction, and at an individual level Shap allows for an understanding of the most influential features in any single prediction.





