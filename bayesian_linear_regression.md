
# <center>Bayesian Linear Regression</center>

## Overview
Bayesian linear regression serves as a simple introduction to Bayesian regression that also happens to be a very widely used Bayesian model.

## Objective
We seek to fit a line $f$ to a set of $d$ covariates $X$ and their targets $Y$. Without loss of generality, assume that $Y$ is 1-dimensional. Let $f$ be paramaterised by $\hat{\theta}$, where $\hat{\theta} = (\theta_{0}, \theta_{1},...,\theta_{d})$. Assume that model residuals are normally distributed with mean $\mu = 0$ and a constant standard deviation $\sigma$. Then $Y$ is modelled with the following equation,

$$f(X; \hat{\theta}) = \hat{\theta}^{T} X + \epsilon$$

Where $\epsilon = \mathcal{N}(0, \sigma)$. The semi-colon notation in $f(X; \hat{\theta})$ indicates that $\theta$ is fixed when evaluating $f(X)$ to determine $Y$. It follows that,

$$ Y \sim \mathcal{N}(\hat{\theta}^{T} X, \sigma)$$

## Choosing Priors
For the prior of $\hat{\theta}$, we keep things simple and choose a multivariate Gaussian prior.

$$\hat{\theta} \sim \mathcal{N}(\mu_{\theta}, \Sigma_{\theta})$$

where $\mu_{\theta} = [0,0,....0]$ and $\Sigma_{\theta} = \lambda I$. Hence

$$p(\hat{\theta}) = \prod_{0}^{d} \mathcal{N}(\hat{\theta} | 0, \lambda) $$

## Calculating the Posterior Distribution
We can calculate the posterior distribution using Bayes' update rule,

$$p(\hat{\theta}|Y;X) = \frac{p( Y| \hat{\theta}; X) p(\hat{\theta})}{\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}}$$

$p(\hat{\theta})$ is already given above. 

$$p( Y| \hat{\theta}; X) = \prod_{n=0}^{N-1} \mathcal{N}(y_{n} | f(x_{n}; \hat{\theta}), \sigma)$$

The integral in the denominator then becomes

$$\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}=\int_{\hat{\theta}}\prod_{n=0}^{N-1}\prod_{0}^{d}\mathcal{N}(y_{n}|f(x_{n,d};\hat{\theta})\mathcal{N}(\hat{\theta}|\mu_{\theta, d},\Sigma_{\theta, d})d\hat{\theta}$$

where $\mu_{\theta, d}$ and $\Sigma_{\theta, d}$ are the mean and standard deviation of the $d^{th}$ component of $\hat{\theta}$.

Integrals like this explain why Bayesian inference is computationally demanding. But using some Google magic, we find that the following solution for the posterior distribution is

$$p(\hat{\theta}|Y;X) = \mathcal{N}(\mu_{\theta}', \Sigma_{\theta}')$$

where

$$\mu_{\theta}' = \frac{1}{\sigma} \Sigma_{\theta}'X^{T}Y$$

and

$$\Sigma_{\theta}' = (\Sigma_{\theta} + \frac{1}{\sigma} X^{T}X)^{-1}$$

## Getting Distribution for Y
To get a distribution of predictions, we use the following equation

$$p(Y|X) = \int_{\theta}p(Y|\theta;X)p(\theta)d\theta $$

again, Google magic gives us the solution

$$ \int_{\theta}p(Y|\theta;X)p(\theta)d\theta = \mathcal{N}(\mu_{\theta}' * X, \sigma+X^{T}\Sigma_{\theta}'X)$$


## Further Reading

https://www.cs.utah.edu/~fletcher/cs6957/lectures/BayesianLinearRegression.pdf

http://www.cs.cmu.edu/~16831-f14/notes/F14/16831_lecture20_jhua_dkambam.pdf

https://cedar.buffalo.edu/~srihari/CSE574/Chap3/3.4-BayesianRegression.pdf