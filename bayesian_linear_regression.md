
# <center>Bayesian Linear Regression</center>

## Overview
Bayesian linear regression serves as a simple introduction to Bayesian regression that also happens to be a very widely used Bayesian model.

## Objective
We seek to fit a line $f$ to a set of $d$ covariates in $n$ samples given by $X$, an $n \times (d+1)$ matrix, and their associated target values $Y$, an $n \times 1$ matrix. Let $f$ be paramaterised by an $1 \times (d+1)$ matrix $\hat{\theta}$. Assume that model residuals are normally distributed with mean $\mu = 0$ and a constant standard deviation $\sigma$. Then $Y$ is modelled with the following equation,

$$f(X; \hat{\theta}) = X\hat{\theta}^{T} + \epsilon$$

$X=\begin{bmatrix} 1& x_{1}\\\vdots& \vdots \\1 & x_{n}\\ \end{bmatrix}$, where each $x_{j}$ represents the covariates of a single sample. This is done to absorb the regression constant into the paramater matrix $\hat{\theta}$. For the remainder of the article $x_{j}$ will *include* the initial $1$ for notational convenience. $\epsilon$ is Gaussian noise with mean $0$ and variance $\sigma^{2}$. It follows that $f(X; \hat{\theta}) \sim \mathcal{N}(X\hat{\theta}^{T}, \sigma)$.

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

$$\mu_{\hat{\theta}}' = \Sigma_{\hat{\theta}}'(\Sigma_{\hat{\theta}}^{-1} \mu_{\hat{\theta}} + X^{T}Y)$$

and

$$\Sigma_{\hat{\theta}}' = (\Sigma_{\hat{\theta}}^{-1} + X^{T}X)^{-1}$$

Dashed variables such as $\mu_{\hat{\theta}}'$ represent updated values, whereas undashed variables such as $\mu_{\hat{\theta}}$ represent variables prior to updating.

The equation for $\mu_{\hat{\theta}}'$ shows us that a Bayesian update weights the new mean of the posterior between the prior mean and the information provided by the new data. The more data there is, the more the update skews the new mean toward the data. Eventually the influence of the original prior is gone altogether. This is counteracted by confidence in the priors, as small standard deviation in the prior distribution enlarges $\Sigma_{\hat{\theta}}^{-1}$, which in turn skews the update toward the prior.

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