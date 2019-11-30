
# <center>Bayesian Linear Regression</center>

## Overview
Bayesian linear regression serves as a simple introduction to Bayesian regression that also happens to be a very widely used Bayesian model.

## Objective
We seek to fit a line $f$ to a set of $d$ covariates $X$ and their targets $Y$. Without loss of generality, assume that $Y$ is 1-dimensional. Let $f$ be paramaterised by $\hat{\theta}$, where $\hat{\theta} = (\theta_{0}, \theta_{1},...,\theta_{d})$. Assume that model residuals are normally distributed with mean $\mu = 0$ and a constant standard deviation $\sigma$. Then $Y$ is modelled with the following equation,

$$f(X; \hat{\theta}) = \hat{\theta}^{T} X + \epsilon$$

Where $\epsilon = \mathcal{N}(0, \sigma)$. The semi-colon notation in $f(X; \hat{\theta})$ indicates that $\theta$ is fixed when evaluating $f(X)$ to determine $Y$. It follows that,

$$ Y \sim \mathcal{N}(\hat{\theta}^{T} X, \sigma)$$

## Choosing Priors
To keep things simple, we choose independant Gaussian priors for each of the model parameters $\theta_{i}$,

$$\theta_{i} \sim \mathcal{N}(\mu_{i}^{\theta}, \sigma_{i}^{\theta})$$

hence

$$p(\hat{\theta}) = \prod_{0}^{d} \mathcal{N}(\mu_{i}^{\theta}, \sigma_{i}^{\theta}) $$

## Calculating the Posterior Distribution
We can calculate the posterior distribution using Bayes' update rule,

$$p(\hat{\theta}|Y;X) = \frac{p( Y| \hat{\theta}; X) p(\hat{\theta})}{\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}}$$

$p(\hat{\theta})$ is already given above. 

$$p( Y| \hat{\theta}; X) = \prod_{n=0}^{N-1} \mathcal{N}(y_{n} | f(x_{n}; \hat{\theta}), \sigma)$$

The integral in the denominator then becomes

$$\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}=\int_{\hat{\theta}} \prod_{n=0}^{N-1}\prod_{0}^{d} \mathcal{N}(y_{n}|f(x_{n};\sigma)\mathcal{N}(\mu_{i}^{\theta},\sigma_{i}^{\theta})d\hat{\theta}$$

## Further Reading

