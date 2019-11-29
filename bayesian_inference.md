
# <center>Introduction to Bayesian Inference</center>

## Overview
Bayesian inference is based on predicting probability distributions for model parameters using Bayes' theorem

$$P(A|B) = \frac{P(B|A) P(A)}{P(B)} $$

### Distribution over point estimates
Suppose we want to estimate the parameters of a predictive model $y = f(x;\theta)$ paramaterised by $\theta$. We can use Bayes' theorem in conjunction with observed data $X, y$ to calculate a probability distribution over $\theta$

$$p(\theta|X, y) = \frac{p( y| \theta, X) p(\theta)}{p(y|X)}$$

Where 
$X$
is a matrix of covariates, 
$y$
are the target values and 
$\beta$ 
is a model parameter. 
$p(\theta)$ 
is called the *prior* distribution of 
$\theta$ 
and 
$p(\theta|X, y)$ 
is called the *posterior* distribution of 
$\theta$.

### Prior Distribution
The prior distribution of $\theta$, $p(\theta)$ (typically referred to simply as a *prior*) represents our prior knowledge of the distribution of $\theta$. Where we have no knowledge, we typically choose a prior 
distribution to be as broad as possible.

### Posterior Distribution
A posterior distribution of $\theta$, $p(\theta|X, y)$ (typically referred to simply as the *posterior*) represents our knowledge of $\theta$ after having observed the data $X, y$. Bayes' theorem provides a way for us to use data to calculate the posterior, given that we have the prior.

### Prediction
Given a distribution over $\theta$ and any given value for $x$, we can calculate a distribution over the values of $f(x;\theta)$ as follows

$$p(y|x) = \int_{\theta}p(y|\theta)p(\theta|x)d\theta $$

In other words, we are taking a weighted average over all possible paramaterisations of $f(x;\theta)$.

### Maximum a Posteriori Estimation
The Bayesian counterpart of maximum likelihood estimation is maximum a posteriori estimation. Here, we incorporate 
the prior into our calculation of the value of $\theta$ that maximises the probability of observing our given data. 

$$ \theta_{MAP} = argmax_{\theta}(L(X, y | \theta) p(\theta)) $$

Where $L$ is the likelihood function and $p(\theta)$ is the prior for $\theta$.

### General Procedure

#### 1. Choose model paramaterisation.
Select a paramaterisation for $f(x;\theta)$. e.g. for linear regression, we choose $f(x;\theta)=\theta x$

#### 2. Choose priors.
Choose an initial distribution for $\theta$. A common choice is a Gaussian distribution $p(\theta) = \frac{1}{\sqrt{2\pi\sigma^{2}}}exp(-\frac{(x - \mu)^2}{2\sigma^{2}}$ with mean $\nu$ and standard deviation $\sigma$.

#### 3. Gather data and use Bayes' theorem to calculate posterior probabilities.


#### 4. Repeat 3 as needed.

### Why use Bayesian inference?

#### More intuitive

#### Gives distribution over model parameters and data

### Why *not* to use Bayesian inference?

#### Computationally intensive




## Further References

