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


