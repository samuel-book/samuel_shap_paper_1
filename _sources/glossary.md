# Glossary of technical terms

## Receiver operating characteristic (ROC) curve

This is a visual tool (in the form of a graph) that shows the tradeoff of choosing different *threshold* values for turning a probability into a classification. It is used to find the threshold value that best balances the number of false alarms with number of undetected positives.

## R-squared

R-squared reports the amount of variability in one feature that can be explained by another feature. It takes range 0 - 1. A R-squared nearing 1 means that the variation in one variable is nearly entirely explainable by the other variable - they are said to be highly correlated. Conversely, a R-squared nearing 0 means that the variation in one variable is nearly not at all explained by the other variable - they are said to be weakly correlated.

## Stratified k-fold cross-valiadation

When dividing your dataset into a train and test set, how much of the result is due to the division of the instances between these two sets? Stratified k-fold cross-validation is a method to divide your data into these two sets a number of times (user defined), such that each instance is only present in the test set once, and the split of instances maintains the balance of values for each feature. By fitting a model to each of these train-test sets allows the model performance to be measured under different splits, and the results robustness can be determined by the consistency of the results.

## Threshold* (for a binary classification model)

Many machine learning models, such as XGBoost, return a probability of recieving thrombolysis, and we use a nominal threshold value to turn this probability into a prediction. By default, this is the midpoint, 0.5. A different threshold can be used. By choosing a lower threshold value (between 0-0.5) fewer instances will be classified as 0 (not recieve thrombolysis) and more will be classified as 1 (recieve thrombolysis). By doing so we will create more false alarms (false positives), but also classify more true positives. By choosing a higher threshold value (between 0.5-1) more instances will be classified as 0 (not recieve thrombolysis) and fewer will be classified as 1 (recieve thrombolysis). By doing so will create more undetected positives (false negatives), but also identify more true negatives. Choosing a threshold value is balancing these two cases.

## ROC AUC (Area under the ROC curve)

This explains the perfomance of the model under the full range of *threshold* values. It is a useful way to compare the performance of different models prior to choosing the threshold value. A perfect model will have ROC AUC of 1. Perfomance of models in this notebook was determined by ROC AUC.

## XGBoost

XGBoost stands for eXtreme Gradient Boost. It is a type of machine learning algorithm which uses many decision trees to arrive at a prediction for an instance. A decision tree is a method of taking all of the instances in the training set and sequentially finding the rule that splits the data into the most homogenous groups based on the target feature. The rule is a feature value, such as, 50 years of age, where those instances greater than 50 yrs will go down one branch, and the under 50s will go down the other. XGBoost ...
