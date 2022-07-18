# Probability, odds, and Shap values (log odds shifts)

Many of us find it easiest to think of the chance of something occurring as a probability. For example, there might be a probability of 10% that it will rain today. That is the same as saying there will be one rainy day out of ten days for days with this given probability of rain.

In our stroke thrombolysis model, Shap values tell us how knowing something particular about a patient (such as 'Is their stroke caused by a clot or a bleed?') adjusts our prediction of whether they will receive thrombolysis or not.

This is made a little more complicated for us because Shap is usually reported as a *log odds shift*. It is useful for us to see how those relate to probabilities, and get a sense of how significant Shap values in the range of 0.5 to 5 (or -0.5 to -5) are, as that is a common range of Shap values that we will see in our models.

## Probability

We will take the example that Shap reports that a model's base probability prediction, before the contribution of features is 0.25, or a 25% probability of receiving thrombolysis; that is 1 in 4 patients with this prediction would be expected to receive thrombolysis.

## Odds

*Probability* expresses the chance of something happening as the number of positive occurrences as a fraction of all occurrences (i.e. the number of patients receiving thrombolysis as a fraction of the total number of patients).

*Odds* express the chance of something happening as the ratio of the number of positive occurrences (i.e. receiving thrombolysis) to the number of negative occurrences (i.e. *not* receiving thrombolysis).

If we have probability prediction of 0.25 would receive thrombolysis, that would mean 1 in 4 of those patients receive thrombolysis. Expressed as odds, for every one patient that receives thrombolysis, three will not. The odds are expressed as 1:3 or 1/3. This may also be calculated as a decimal (1 divided by 3), 0.333.

Odds (O) and probability (P) may be converted with the following equations:

(1) O = P / (1 - P)

(2) P = O / (1 + O)

## Log odds shifts

Here we will calculate the effect of Shap values, and try and build some intuition on the size of effect Shap values of 0.5 to 5 give (we will look at positive and negative Shap values).

Shap usually outputs the effect of a particular feature in how much it shifts the odds. For reasons we will not go into here, that shift (which is the 'Shap value') is usually given in 'log odds' (the logarithm of the odds value). For the mathematically inclined, we use the natural log (*ln*).

Let's look at some Shap values (log odds) and see how much they change the odds of receiving thrombolysis. 

First we'll look at the shift in odds the Shap values give. This is calculated as *shift = exp(Shap)*

| Shap (log odds) | Shift in odds (multiply original odds) |
|-----------------|----------------------------------------|
| 0.5             | 1.65                                   |
| 1               | 2.72                                   |
| 2               | 7.39                                   |
| 3               | 20.1                                   |
| 4               | 54.6                                   |
| 5               | 148                                    |

### Positive Shap values: worked example

Now let us work through an example of starting with a known *probability*, converting that to *odds*, applying a Shap *log odds shift* for a particular feature, and converting back to *probability* after we have applied the influence of that feature.

Here are the effects of those shifts on our baseline probability of 0.25.

| Starting P | Starting O | Shap | Shift (multiply O) | Shifted O |  Shifted P (%) |
|------------|------------|------|--------------------|-----------|----------------|
| 0.25 (25%) | 0.333      | 0.5  | 1.65               | 0.550     | 0.3547 (35.5%) |
| 0.25 (25%) | 0.333      | 1    | 2.72               | 0.907     | 0.4754 (47.5%) |
| 0.25 (25%) | 0.333      | 2    | 7.39               | 2.46      | 0.7112 (71.1%) |
| 0.25 (25%) | 0.333      | 3    | 20.1               | 6.70      | 0.8700 (87.0%) |
| 0.25 (25%) | 0.333      | 4    | 54.6               | 18.2      | 0.9479 (94.8%) |
| 0.25 (25%) | 0.333      | 5    | 148                | 49.5      | 0.9802 (98.0%) |

So, for example, a Shap value of 0.5 for one particular feature tells us that that particular feature in that patient shifts our expected probability of that patient receiving thrombolysis from 25% to 36%. A Shap value of 5 for the same feature would shift the probability of that patient receiving thrombolysis up to 98%.

### Negative Shap values: worked example

If we have a negative Shap value then odds are reduced (a Shap of -1 will lead to the odds being divided by 2.72, which is the same as multiplying by 1/2.72, which is 0.3679):

| Starting P | Starting O | Shap | Shift (multiply O) | Shifted O |   Shifted P    |
|------------|------------|------|--------------------|-----------|----------------|
| 0.25 (25%) | 0.333      | -0.5 | 0.6065             | 0.2022    | 0.1682 (16.8%) |
| 0.25 (25%) | 0.333      | -1   | 0.3679             | 0.1226    | 0.1092 (10.9%) |
| 0.25 (25%) | 0.333      | -2   | 0.1353             | 0.0451    | 0.0432 (4.32%) |
| 0.25 (25%) | 0.333      | -3   | 0.0498             | 0.0166    | 0.0163 (1.63%) |
| 0.25 (25%) | 0.333      | -4   | 0.0183             | 0.0061    | 0.0061 (0.61%) |
| 0.25 (25%) | 0.333      | -5   | 0.0067             | 0.0022    | 0.0022 (0.22%) |

So, for example, a Shap value of -0.5 for one particular feature tells us that that particular feature in that patient shifts our expected probability of that patient receiving thrombolysis from 25% to 17%. A Shap value of 5 for the same feature would shift the probability of that patient receiving thrombolysis down to 2%.

### Observations about Shap values

We begin to get some intuition on Shap values. A Shap value of 0.5 (or -0.5) leads to a small, but still noticeable, change in probability. Shap values of 5 or -5 have effectively pushed probabilities to one extreme or the other.







