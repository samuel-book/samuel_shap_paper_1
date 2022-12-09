# Summary and outline

Part of the NIHR *SAMueL* (Stroke Audit Machine Learning) project.

## Background

Stroke is a common cause of adult disability. Most strokes (about four out of five) are caused by a blood clot in the brain, and have the potential to be treated with clot-busting drugs to break up the blood clot that is causing their stroke  - this is called *thrombolysis*. Thrombolysis improves stroke outcomes overall, with more people being able to carry out their normal daily activities. There is, however, a small risk of a bleed in the brain, which is fatal in about 1 in 50 patients receiving thrombolysis, with the risk bing highest in those with the most severe strokes. Overall, thrombolysis does not increase risk of death, as the risk of death from a bleed is balanced out by the benefits of thrombolysis to others. Clinicians, patients, and carers, must however consider both benefits and risks of thrombolysis when deciding on whether to use it.

Expert opinion is that about one in five patients should receive thrombolysis, and this is the target set in the NHS long term plan. At the moment only about one in nine patients actually receive this treatment in the UK. There is a lot of variation between hospitals, which means that the same patient might receive different treatment depending on which hospital they attend ({numref}`figure {number} <thrombolysis_hist>`). 

:::{figure-md} thrombolysis_hist
<img src="./images/thrombolysis_by_hospital.jpg" width="450">

Variation in thrombolysis use across hospitals in England and Wales 2016-2018. 
:::

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

> Shapley values (or *SHAP*, SHapley Additive exPlanations, values, which is a particular method of estimating Shapley values) are *'the average expected marginal contribution of one player after all possible combinations have been considered'*.

Imagine a pub quiz team with up to 3 people. Any number of people may actually turn up on the night:

* There are 8 possible combinations of players (including no-one turning up).

* The Shapley value for any team member describes the average difference in score when a particular player is present or absent compared to the average of all combinations of players.

The same principle may be applied in machine learning: How does any one feature (e.g. stroke severity, or age), on average, contribute to the prediction after considering all possible combinations of features? What difference does that feature make to the prediction?

## Key findings

### Predicting thrombolysis use with an XG-Boost model

Looking at patterns oh SHAP values reveals the following:

* Stroke type: As expected,  the SHAP values for stroke types effectively eliminates any chance of receiving thrombolysis for haemorrhagic stroke.

* Arrival-to-scan time: The odds of receiving thrombolysis reduces by about 20 fold over the first 100 minutes of arrival to scan time.

* Stroke severity (NIHSS): The odds of receiving thrombolysis i slowest at NIHSS 0, rises and peaks at NIHSS 15-25, and then falls again with higher stroke severity. The difference between minimum odds (at NIHSS 0) and maximum odds (at 15-25) of receiving thrombolysis is 30-35 fold.

* Stroke onset time type (precise vs. estimated): The odds of receiving thrombolysis are about 3 fold greater for precise onset time than estimated onset time.

* Disability level (Rankin) before stroke. The odds of receiving thrombolysis falls about 5 fold between mRS 0 and 5.

* The hospital SHAP values ranges from -1.4 to +1.4. This range of SHAP (log odds) represents a 15 fold difference in odds of receiving thrombolysis (most are in the range of -1 to +1, but this still represents a 7-8 fold difference in odds of receiving thrombolysis). 

{numref}`Figure {number} <SHAP_violin>` shows a violin plot of SHAP values for six features.

:::{figure-md} SHAP_violin
<img src="./images/xgb_thrombolysis_shap_violin.jpg" width="800">

A violin plot showing the individual SHAP values for six features. The shape of the *violin* shows the spread of the size of SHAP values for each feature value. A positive SHAP vales pushes the model towards saying that patient would *would* receive thrombolysis. A negative SHAP value pushes the model towards saying that patient would would *not* receive thrombolysis.
:::

SHAP plots can also be used to explain predictions of any individual patient (e.g. {numref}`Figure {number} <waterfall_example>`). 

:::{figure-md} waterfall_example
<img src="./images/xgb_waterfall_low_probability.jpg" width="800">

An example of a SHAP *waterfall* plot showing the most influential features in influencing the model's prediction of a patient probability of receiving thrombolysis (in this case a patient with a very low probability of receiving thrombolysis). In this example the three most influential features, reducing the chance of receiving thrombolysis were 1) low stroke severity (NIHSS 2), 2) slow arrival-to-scan time (138 mins), and 3 the hospital attended (stroke team LFPMM4706C).
:::

### Comparing hospital SHAP values with the predicted thrombolysis rate at each hospital if all hospitals saw the same 10k cohort of patients

We can assess each hospital's *'propensity to use thrombolysis'* by passing the same 10k cohort of patients through all hospital prediction models (by keeping all patient features the same apart from changing the hospital ID). In this analysis we train the XGBoost model on all patients apart from those in the 10k patient cohort (which are selected randomly from the full data set), and then assess thrombolysis use in the 10k data set.

When we compare this 10k thrombolysis rate to the average hospital SHAP model in our previously trained XGBoost model ({numref}`Figure {number} <shap_vs_10k>`), we find a very strong correlation (R-squared = 0.917). This helps to validate average hospital SHAP being used as a measure of a hospital's *'propensity to use thrombolysis'*.

:::{figure-md} shap_vs_10k
<img src="./images/shap_vs_10k.jpg" width="450">

A comparison of average hospital SHAP values with predicted hospital thrombolysis use if all hospitals saw the same 10k patient cohort.
:::

We see that hospital SHAP values range from about -1.5 to +1.5. This range of 3 (in log odds) between hopsitals represents a range of about 20 fold in the odds of a patient receiving thrombolysis, simply by virtue of which hospital they attend. Most hospitals lie within the range of -1.0 to +1.0, but this still represents a 7-8 fold range in the ods of receiving thrombolysis.

### How much of the variation in thrombolysis use (for patients arriving within 4 hours of known stroke onset) can be explained by the hospital SHAP value? 

We can also look at how a hospital's SHAP value correlates with the thrombolysis rate for patients attending each hospital (focussing on patients arriving within 4 hours of stroke onset). We see that the hospital's SHAP value explains 58% of the variance in thrombolysis rate of patients ({numref}`Figure {number} <hosp_shap>`). This suggests that, differences in decisions on whether and when to use thrombolysis dominates differences in patient mix or difference in key process times (time to arrival, and time from arrival to scan).

:::{figure-md} hosp_shap
<img src="./images/hospital_shap_own_patients.jpg" width="450">

A comparison of average hospital SHAP values with actual hospital thrombolysis use for patients arriving within 4 hours of known stroke onset.
:::


### What do SHAP interactions reveal about differences between hospitals

SHAP interactions show how one feature may modify the effect of another feature. We are especially interested in how an individual hospital may modify the general effects observed in the model, as this reveals patterns of decisions in individual hospitals. Below are three examples where hospitals make different decisions around stroke severity, disability before stroke, and whether stroke onset time is known precisely or not.

#### Stroke severity

{numref}`Figure {number} <nihss_main>` shows the main effect of stroke severity is to significantly reduce the odds of receiving thrombolysis for mild strokes (NIHSS 0-5), increase the odds of receiving thrombolysis for more moderate to sever strokes (NIHSS 6-32), and then reduce the odds of receiving thrombolysis for very severe stroke strokes (NIHSS 33+).

:::{figure-md} nihss_main
<img src="./images/12ab_nihss_main_effect.jpg." width="450">

The main effect of stroke severity on the odds of receiving thrombolysis (SHAP shows the adjustment of log odds).
:::

Individual hospitals, in addition to having their own independent SHAP value, may then specifically adjust this general pattern. {numref}`Figure {number} <nihss_interaction>` shows two stroke teams with opposite effects, one that opposes the general effect of stroke severity, and one that strengthens it.

:::{figure-md} nihss_interaction
<img src="./images/12ab_stroke_severity_interaction_example.jpg." width="450">

The interaction between stroke team and the SHAP value of stroke severity (the interaction value is added to the main stroke severity effect). *team_KECZG3964M* has a SHAP interaction that opposes the general stroke severity main effect, especially increasing the odds of receiving thrombolysis for mild strokes. *team_SMVTP6284P* strengthens the main effect of stroke severity - reducing the odds of receiving thrombolysis even further for mild strokes.
:::


#### Disability before stroke

{numref}`Figure {number} <mrs_main>` shows the main effect of prior disability - that is to progressively reduce the odds of receiving thrombolysis with increasing disability (a SHAP of +0.3 for mrS=0 down to -1.50 for mRS=5)

:::{figure-md} mrs_main
<img src="./images/12ac_prior disability level_main_effect.jpg." width="450">

The main effect of pre-stroke disability on the odds of receiving thrombolysis (SHAP shows the adjustment of log odds).
:::

Individual hospitals, in addition to having their own independent SHAP value, may then specifically adjust this general pattern. {numref}`Figure {number} <mrs_interaction>` shows two stroke teams with opposite effects, one that opposes the general effect of pre-stroke disability, and one that strengthens it.


:::{figure-md} mrs_interaction
<img src="./images/12ac_disability_interaction_example.jpg." width="450">

The interaction between stroke team and the SHAP value of pre-stroke disability (the interaction value is added to the main disability effect). *team_XKAWN3771U* has a SHAP interaction that opposes the general prior disabilityt main effect. *team_AKCGO9726K* strengthens the main effect of stroke severity - reducing the odss of receiving thrombolysis even futher for mild strokes.
:::


#### Stroke onset time

The main effect of whether stroke onset time is known precisely or not is as follows (with SHAP giving the adjustment to the log odds of receiving thrombolysis):

* If stroke onset time is known precisely: SHAP = +0.42
* If stroke onset time is *not* known precisely: SHAP = -0.85

Individual hospitals, in addition to having their own independent SHAP value, may then specifically adjust this general pattern. {numref}`Figure {number} <onset_interaction>` shows two stroke teams with opposite effects, one that opposes the general effect of pre-stroke disability, and one that strengthens it.

:::{figure-md} onset_interaction
<img src="./images/12aa_onset_time_type_interaction_example.jpg." width="450">

The interaction between stroke team and the SHAP value of precisely known onset time (the interaction value is added to the main onset effect). *team_HZNVT9936G* has interactions that strengthen the effect of *precise onset time* whereas *team_HZNVT9936G* has interactions values that attenuate the main effect of *precise onset time*
:::


## Conclusions

*Explainable machine learning* techniques give significant insight into models prediction clinical decision-making. At an overall level, SHAP allows for an understanding of the relationship between feature values and the model prediction, and at an individual level SHAP allows for an understanding of the most influential features in any single prediction.
