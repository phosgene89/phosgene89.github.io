
# <center>Bayesian Linear Regression</center>

## Overview
Bayesian inference regards model parameters as random variables, rather than their point estimates as random variables.

## Bayesian Inference Overview
Bayesian inference is based on predicting probability distributions for model parameters using Bayes' theorem

$$P(\beta|X,y) = \frac{P(y|\beta, X) P(\beta|X)}{P(y|X)} $$

Where $\beta$ is a model parameter, $X$ is an array of input data and $y$ contains the target values.

### Priors
Priors represent our prior knowledge of the target variables. Where we have no knowledge, we typically choose a prior distribution to be as broad as possible.

### Distribution over point estimates
We can use Bayes' theorem in conjunction with observed data $X, y$ to calculate a probability distribution over model parameters $\theta$

$$p(\theta|X, y) = \frac{p(y|\theta, X) p(\theta, X)}{p(y|X)}$$

### Prediction
We can then predict the 

## Why do Bayesian inference?

### Procedure

#### 1. Choose model paramaterisation.


#### 2. Choose priors.


#### 3. Gather data and use Bayes' theorem to calculate posterior probabilities.


#### 4. Repeat 3 as needed.




# Further References

