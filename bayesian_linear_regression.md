
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

where $\mu_{\theta} = [0,0,....0]$ and $\Sigma_{\theta} = \lambda I$.

hence

$$p(\hat{\theta}) = \prod_{0}^{d} \mathcal{N}(\hat{\theta} | 0, \lambda) $$

## Calculating the Posterior Distribution
We can calculate the posterior distribution using Bayes' update rule,

$$p(\hat{\theta}|Y;X) = \frac{p( Y| \hat{\theta}; X) p(\hat{\theta})}{\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}}$$

$p(\hat{\theta})$ is already given above. 

$$p( Y| \hat{\theta}; X) = \prod_{n=0}^{N-1} \mathcal{N}(y_{n} | f(x_{n}; \hat{\theta}), \sigma)$$

The integral in the denominator then becomes

$$\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}=\int_{\hat{\theta}}\prod_{n=0}^{N-1}\prod_{0}^{d}\mathcal{N}(y_{n}|f(x_{n,d};\hat{\theta})\mathcal{N}(\hat{\theta}|0,\lambda)d\hat{\theta}$$

Using some Google magic, we find that the above integral evaluates to

$$\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta} = \mathcal{N}(X^{T}\mu_{\theta}, \Sigma_{X})$$

where 

$$\Sigma_{X} = \sigma^{2} + X^{T}\Sigma_{\theta}X$$



testestest

test
test

test

$$\prod_{n=0}^{N-1}\prod_{0}^{d}\mathcal{N}(y_{n}|f(x_{n,d};\hat{\theta})\mathcal{N}(\hat{\theta}|0,\lambda)=\mathcal{N}(y_{n}|\mu_{\theta}, \Sigma_{\theta})$$

where

$$\mu_{\theta} = (X^{T}X + \sigma\Lambda)^{-1}X^{T}Y$$

and

$$\Sigma_{\theta} = \sigma^{2}(X^{T}X + \sigma^{2}\Lambda)^{-1}$$

## Further Reading

http://www.biostat.umn.edu/~ph7440/pubh7440/BayesianLinearModelGoryDetails.pdf

https://www.cs.princeton.edu/~bee/courses/lec/lec_jan31.pdf
