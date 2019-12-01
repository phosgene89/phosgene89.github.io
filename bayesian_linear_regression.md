
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

$$\hat{\theta} \sim \mathcal{N}(\mu_{\hat{\theta}}, \Sigma_{\hat{\theta}})$$

where $\mu_{\hat{\theta}} = [0,0,....0]$ and $\Sigma_{\hat{\theta}} = \lambda I$.

## Calculating the Posterior Distribution
We can calculate the posterior distribution using Bayes' update rule,

$$p(\hat{\theta}|Y;X) = \frac{p( Y| \hat{\theta}; X) p(\hat{\theta})}{\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}}$$

$p(\hat{\theta})$ 
is already given above. 
$p(Y|\hat{\theta}; X)$ 
is given by

$$p( Y| \hat{\theta}; X) = \prod_{n=0}^{N-1} \mathcal{N}(y_{n} | f(x_{n}; \hat{\theta}), \sigma)$$

The integral in the denominator then becomes

$$\int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta}=\int_{\hat{\theta}}\prod_{n=0}^{N-1}\prod_{0}^{d}\mathcal{N}(y_{n}|f(x_{n,d};\hat{\theta})\mathcal{N}(\hat{\theta}|\mu_{\hat{\theta}, d},\Sigma_{\hat{\theta}, d})d\hat{\theta}$$

where $\mu_{\theta, d}$ and $\Sigma_{\theta, d}$ are the mean and standard deviation of the $d^{th}$ component of $\hat{\theta}$.

Integrals like this explain why Bayesian inference is computationally demanding. But using some Google magic, we find that the <a href ="https://cedar.buffalo.edu/~srihari/CSE574/Chap3/3.4-BayesianRegression.pdf">solution</a> (courtesy of our friends over at CEDAR in the University at Buffalo, NY) for the posterior distribution is

$$p(\hat{\theta}|Y;X) = \mathcal{N}(\mu_{\hat{\theta}}', \Sigma_{\hat{\theta}}')$$

where

$$\mu_{\hat{\theta}}' = \Sigma_{\hat{\theta}}'(\Sigma_{\hat{\theta}}^{-1} \mu_{\hat{\theta}} + X^{T}X\hat{\theta})$$

and

$$\Sigma_{\hat{\theta}}' = (\Sigma_{\hat{\theta}}^{-1} + X^{T}X)^{-1}$$



Dashed variables such as $\mu_{\hat{\theta}}'$ represent updated values, whereas undashed variables such as $\mu_{\hat{\theta}}$ represent variables prior to updating.

The equation for $\mu_{\hat{\theta}}'$ shows us that a Bayesian update weights the new mean of the posterior between the prior mean and the information provided by the new data. The more data there is, the more the update skews the new mean toward the data. Eventually the influence of the original prior is gone altogether.

$\Sigma_{\hat{\theta}}'$ *decreases* with each update, due to us becoming more and more confident of where $\mu_{\hat{\theta}}'$ actually is.

## Getting Distribution for Predictions
To get a distribution of predictions, we use the following equation

$$p(Y|X) = \int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta} $$

again, Google magic gives us <a href ="https://cedar.buffalo.edu/~srihari/CSE574/Chap3/3.4-BayesianRegression.pdf">the solution</a> (again, courtesy of our friends over at CEDAR in the University at Buffalo, NY)

$$ \int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta} = \mathcal{N}(\mu_{\hat{\theta}}'X, \sigma+X^{T}\Sigma_{\hat{\theta}}'X)$$


## Further Reading

<a href="https://en.wikipedia.org/wiki/Bayesian_linear_regression">Bayesian linear regression | Wikipedia</a>

<a href="https://cedar.buffalo.edu/~srihari/CSE574/Chap3/3.4-BayesianRegression.pdf">Bayesian linear regression | CEDAR, University at Buffalo, NY</a>

<a href="https://www.cs.utah.edu/~fletcher/cs6957/lectures/BayesianLinearRegression.pdf">Notes on Bayesian linear regression | School of Computing, University of Utah</a>

<a href="http://www.cs.cmu.edu/~16831-f14/notes/F14/16831_lecture20_jhua_dkambam.pdf">Bayesian linear regression | Statistical techniques in robotics, Carnegie Mellon University</a>