# Introduction

*Applying explainable machine learning to national stroke audit data to explore variation in decisions to use thrombolysis* (part of the NIHR *SAMueL*, Stroke Audit Machine Learning, project).

Kerry Pearn & Michael Allen

```{epigraph}
"Your decision to treat or not treat … That’s the difficult part. That’s the grey area where everyone does a different thing."

-- Stroke Consultant during interviews for SAMueL
```

## Background

Stroke is a common cause of adult disability. Most strokes (about four out of five) are caused by a blood clot in the brain, and have the potential to be treated with clot-busting drugs to break up the blood clot that is causing their stroke  - this is called *thrombolysis*. Thrombolysis improves stroke outcomes overall, with more people being able to carry out their normal daily activities. There is, however, a small risk of a bleed in the brain, which is fatal in about 1 in 50 patients receiving thrombolysis, with the risk bing highest in those with the most severe strokes. Overall, thrombolysis does not increase risk of death, as the risk of death from a bleed is balanced out by the benefits of thrombolysis to others. Clinicians, patients, and carers, must however consider both benefits and risks of thrombolysis when deciding on whether to use it.

Expert opinion is that about one in five patients should receive thrombolysis, and this is the target set in the NHS long term plan. At the moment only about one in nine patients actually receive this treatment in the UK. There is a lot of variation between hospitals, which means that the same patient might receive different treatment depending on which hospital they attend.

In a previous project, [SAMueL-1](https://samuel-book.github.io/samuel-1/introduction/intro.html), we trained machine-learning models to predict whether any individual patient would receive thrombolysis in any hospital. This allowed us to investigate what differences in treatment are likely to be due to differences between patients, and what differences in treatment were likely to be due to differences between hospitals.

## Aims of this study

The aims of this study were 1) to apply *explainable machine learning* techniques to investigate the most significant features that drive decisions to use thrombolysis at different hospitals, and 2) to model and explain what are the the features that are most important in hospitals that we predict would make *different* decisions about any given patient.

## What is *Explainable Machine Learning*?

Machine learning models generally learn from large sets of data - learning patterns between aspects of the data and some outcome of interest. In this case the data contains a range of *features* about the patient, such as their age, sex, a breakdown of their stroke symptoms, etc. The machine learning model learns the relationship between those features and whether the patient receives thrombolysis or not. 

A general diagram of our machine learning is shown in {numref}`Figure {number} <high_level_md>`. 

:::{figure-md} high_level_md
<img src="./images/ml_model_high_level.png" width="600">

A general depiction of machine learning models trained to predict use of thrombolysis for any patient given 1) the hospital they attend, 2) patient and clinical information, and 3) pathway and process information. 
:::

There are many different types of machine learning (here we use one called *XG-Boost*), but all are making predictions based on similarities to what the model has seen before. Many machine learning models are what we call a *black box* model - that is we give it some information, and it makes a prediction, but we don't know *why* it made that particular prediction. 

*Explainable Machine Learning* seeks to be able to communicate *why* a model makes the prediction it does. We seek to understand, and communicate, the general patterns that the model is making (we call this *global explainability*), as well as why the model made the prediction it did for one particular patient (we call this *local explainability*). We also try to explain other important aspects about the model such as where the training data came from (and how representative is that data of where the model will be used in practice), and how sure we can be of the model's predictions - both generally and for any particular prediction.

In this project we are very much on a journey - discovering what different people would like to know about the model. Do patients, carers, clinicians, and other machine learning researchers all want to know the same things, or different things? How can we tailor *explainable machine learning* output to the wishes of different audiences?

(*Explainable machine learning* may also be known as *Explainable ML*, *Explainable artificial intelligence*, or *Explainable AI*).

## Methods

In this study we used a machine learning method called *XG-Boost* to predict decisions to give thrombolysis at each of 132 hospitals in England and Wales that deal with emergency stroke admissions. 

In order to make the model easier to explain, we found the most important features that would predict whether a patient received thrombolysis or not. We found that with just 8 features we could get accuracy that was very close to using *all* available features. These 8 features were:

* *S2BrainImagingTime_min*: Time from arrival at hospital to scan
* *S2StrokeType_Infarction*: Stroke type: clot ('infarction') or bleed ('haemorrhage')
* *S2NihssArrival*: Stroke severity (National Institutes of Health Stroke Scale; NIHSS) on arrival
* *S1OnsetTimeType_Precise*: Is stroke onset time known precisely (or estimated)
* *S2RankinBeforeStroke*: Disability level (modified Rankin Scale) *before* stroke
* *StrokeTeam*: Hospital ID
* *AFAnticoagulent_Yes*: Patient on anticoagulant therapy for atrial fibrillation
* *S1OnsetToArrival_min*: Time from stroke onset to arrival at hospital

Note: The [GitHub repository](https://github.com/samuel-book/samuel_shap_paper_1) also includes XG-Boost models using all available features.

In order to explain model predictions we used a method called Shapley values, which are described below.

### What are Shapley values?

> Shapley values (or *Shap* values)are *'the average expected marginal contribution of one player after all possible combinations have been considered'*.

Imagine a pub quiz team with up to 3 people. Any number of people may actually turn up on the night:

* There are 8 possible combinations of players (including no-one turning up).

* The Shapley value for any team member describes the average difference in score when a particular player is present or absent compared to the average of all combinations of players.

The same principle may be applied in machine learning: How does any one feature (e.g. stroke severity, or age), on average, contribute to the prediction after considering all possible combinations of features? What difference does that feature make to the prediction?

## Key findings

### Predicting thrombolysis use with an XG-Boost model

The five most influential features predicting whether thrombolysis would be given or not were (in order of importance):

1. *Stroke type (infarction vs. haemorrhage)*: Use of thrombolysis depended on it being an infarction (clot).
2. *Time from arrival at hospital to time brain imaging was performed*: Predicted probability of using thrombolysis reduced with increasing time to scan.
3. *Stroke severity (NIHSS) on arrival*: Predicted probability of using thrombolysis was low with very mild strokes,rose with increasing severity with a plateau at about NIHSS of 10-20, and then reduced with the most severe strokes.
4. *Stroke onset time type (precise vs. estimated)*: Predicted probability of using thrombolysis increased with a precisely known  onset.
5. *Disability level (Rankin) before stroke*: Predicted probability of using thrombolysis reduced with increasing disability before stroke.

Shap plots could be used to explain predictions of any individual patient (e.g. {numref}`Figure {number} <waterfall_example>`). 

:::{figure-md} waterfall_example
<img src="./images/xgb_waterfall_low_probability.png" width="800">

An example of a Shap *waterfall* plot showing the most influential features in influencing the model's prediction of a patient probability of receiving thrombolysis (in this case a patient with a very low probability of receiving thrombolysis). In this example the three most influential features, reducing the chance of receiving thrombolysis were 1) slow arrival-to-scan time (138 mins), low stroke severity (NIHSS 2), and 3 the hospital attended.
:::

### Predicting *differences* in thrombolysis use between hospitals with an XG-Boost model

We trained an XG-Boost model to predict different choices in thrombolysis between units with a high or low propensity to use thrombolysis. Using this model we found that lower thrombolysing hospitals were less likely to give thrombolysis...

1. With increasing disability before stroke.
2. In milder strokes.
3. When stroke onset time had been estimated (rather than known precisely).
4. With longer onset-to-arrival times.
5. With longer arrival-to-scan times.


## Conclusions

*Explainable machine learning* techniques give significant insight into models prediction clinical decision-making. At an overall level, Shap allows for an understanding of the relationship between feature values and the model prediction, and at an individual level Shap allows for an understanding of the most influential features in any single prediction.





