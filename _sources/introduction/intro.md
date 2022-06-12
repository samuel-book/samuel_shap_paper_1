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
<img src="./images/ml_model_high_level.png" width="800px">

A high level depiction of machine learning models trained to predict use of thrombolysis for any patient given 1) the hospital they attend, 2) patient and clinical information, and 3) pathway and process information.
:::


