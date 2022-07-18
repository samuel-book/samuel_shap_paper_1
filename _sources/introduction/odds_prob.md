# Probability, odds and Shap log odds

Let's take an example relevant to our work here with Shap.

## Probability

We will take the example that Shap reports that a model's base probability prediction, before the contribution of features is 0.25, or a 25% probability of receviving thrombolysis; that is 1 in 4 patients with this prediction would be expected to receive thrombolysis.

## Odds

Probability expresses the chance of something happening as the number of positive occurrence as a fraction of all outcomes.

Odds express the chance of something happening as the ratio of the positive (i.e. receiving thrombolysis), and negative ( (i.e. *not* receiving thrombolysis) classes.

If we have probability prediction of 0.25 would receive thrombolysis, that would mean 1 in 4 of those patients receive thrombolysis. Or, expressed as odds, for every patient that receives thrombolysis, three will not. The odds are expressed as 1:3 or 1/3. This may also be calculated as a decimal, 0.333.

Odds (O) and probability (P) may be converted with the following equations:

(1) O = P / (1 - P)

(2) P = O / (1 + O)

## Log odds shifts

Here we will calculate the effect of Shap values, and try and build some intuition on the size of effect Shap values of 1 to 5 give.

Shap usually outputs the effect of a particular feature in how much it shifts the odds. For reasons we will not go into here, that shift (which is the 'Shap value') is usually given in log odds. For the mathematically inclined, we use the natural log (ln).

Let's look at three Shap values (log odds) and see how much they change the probability of receiving thrombolysis. 

First we'll look at the shift the Shap values give. This is calculated as *shift = exp(Shap)*

| Shap (log odds) | Shift (multiply original odds) |
|-----------------|--------------------------------|
| 1               | 2.72                           |
| 2               | 7.39                           |
| 3               | 20.1                           |
| 4               | 54.6                           |
| 5               | 148                            |

Here are the effects of those shifts on our baseline probability of 0.25.

| Starting P | Starting O | Shap | Shift (multiply O) | Shifted O | Shifted P |
|------------|------------|------|--------------------|-----------|-----------|
| 0.25       | 0.333      | 1    | 2.72               | 0.906     | 0.475     |
| 0.25       | 0.333      | 2    | 7.39               | 2.463     | 0.711     |
| 0.25       | 0.333      | 3    | 20.09              | 6.695     | 0.870     |
| 0.25       | 0.333      | 4    | 54.60              | 18.199    | 0.948     |
| 0.25       | 0.333      | 5    | 148.41             | 49.471    | 0.980     |


If we have a negative Shap value then odds are reduced (a Shap of -1 will lead to the odds being divided by 2.72, which is the same as multiplying by 1/2.72, which is 0.3679):

| Starting P | Starting O | Shap | Shift (multiply O) | Shifted O | Shifted P |
|------------|------------|------|--------------------|-----------|-----------|
| 0.25       | 0.333      | -1   | 0.3679             | 0.1226    | 0.1092    |
| 0.25       | 0.333      | -2   | 0.1353             | 0.0451    | 0.0432    |
| 0.25       | 0.333      | -3   | 0.0498             | 0.0166    | 0.0163    |
| 0.25       | 0.333      | -4   | 0.0183             | 0.0061    | 0.0061    |
| 0.25       | 0.333      | -5   | 0.0067             | 0.0022    | 0.0022    |

We begin to get some intuition on Shap values. A Shap value of 1 (or -1) leads to a noticeable change in probability. Shap values of 5 or -5 have effectively pushed probabilities to one extreme or the other.





