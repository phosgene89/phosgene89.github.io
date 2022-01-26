
# <center>Bayesian Linear Regression</center>

## Overview
Bayesian linear regression serves as a simple introduction to Bayesian regression that also happens to be a very widely used Bayesian model.

## Objective
We seek to fit a line to a set of $d$ covariates in $n$ samples. We want our answer to come in the form of a distribution over possible solutions, to give us an idea of how confident we are in the weight estimates of our model.

Let our input features be given by $X$, an $n \times (d+1)$ matrix, and their associated target values $Y$, an $n \times 1$ matrix. Let $\hat{\theta}$ be a $1 \times (d+1)$ matrix representing model weights. We assume that 

$$Y = X\hat{\theta}^{T} + \epsilon$$

where $\epsilon=\mathcal{N}(0, \sigma)$ and $\sigma$ is constant. It follows that $Y \sim \mathcal{N}(X\hat{\theta}^{T}, \sigma)$.

Note that $X$ is given by  $X=\begin{bmatrix} 1& x_{1}\\\vdots& \vdots \\1 & x_{n}\\ \end{bmatrix}$, where each $x_{j}$ represents the covariates of a single sample. This is done to absorb the regression constant into the paramater matrix $\hat{\theta}$. For the remainder of the article $x_{j}$ will *include* the initial $1$ for notational convenience.

## Inferring Model Parameters
We will use Bayesian inference to find a distribution over values of $\hat{\theta}$ that we can use to choose optimal weights and quantify our confidence in how correct they are.

The steps are as follows:

1. Define a prior distribution over model weights $\hat{\theta}$.
2. Use Bayes' update rule along with our data $(X, Y)$ to calculate the posterior distribution over model weights.

If at any point we obtain more data samples, we can improve our estimated distribution by repeating step 2.

## Choosing Priors
For the prior of $\hat{\theta}$, we keep things simple and choose a multivariate Gaussian prior.

$$\hat{\theta} \sim \mathcal{N}(\mu_{\hat{\theta}}, \Sigma_{\hat{\theta}})$$

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

Integrals like this illustrate why Bayesian inference is computationally demanding. In this case, the solution can be solved analytically, but for more complex cases approximations and/or numerical methods are required. Using some Google magic, we find that the solution (<a href ="https://cedar.buffalo.edu/~srihari/CSE574/Chap3/3.4-BayesianRegression.pdf">taken from this link</a>) is:

$$p(\hat{\theta}|Y;X) = \mathcal{N}(\mu_{\hat{\theta}}', \Sigma_{\hat{\theta}}')$$

where

$$\mu_{\hat{\theta}}' = \Sigma_{\hat{\theta}}'(\Sigma_{\hat{\theta}}^{-1} \mu_{\hat{\theta}} + X^{T}Y)$$

and

$$\Sigma_{\hat{\theta}}' = (\Sigma_{\hat{\theta}}^{-1} + X^{T}X)^{-1}$$

Dashed variables such as $\mu_{\hat{\theta}}'$ represent updated values, whereas undashed variables such as $\mu_{\hat{\theta}}$ represent variables prior to updating.

The equation for $\mu_{\hat{\theta}}'$ shows us that a Bayesian update weights the new mean of the posterior between the prior mean and the information provided by the new data. The more data there is, the larger the resulting elements of $X^{T}Y$ will be. Therefore, as $n$ grows larger, $\mu_{\hat{\theta}}'$ becomes increasingly dominated by the data term $X^{T}Y$. Eventually the influence of the original prior is gone altogether. This is counteracted by confidence in the priors, as small standard deviation in the prior distribution enlarges $\Sigma_{\hat{\theta}}^{-1}$, which in turn skews the update toward the prior.

$\Sigma_{\hat{\theta}}'$ *decreases* with each update, due to us becoming more and more confident of where $\mu_{\hat{\theta}}'$ actually is.

## Getting Distribution for Predictions
To get a distribution of predictions, we use the following equation

$$p(Y|X) = \int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta} $$

again, Google magic gives us <a href ="https://cedar.buffalo.edu/~srihari/CSE574/Chap3/3.4-BayesianRegression.pdf">the solution</a>:

$$ \int_{\hat{\theta}}p(Y|\hat{\theta};X)p(\hat{\theta})d\hat{\theta} = \mathcal{N}(\mu_{\hat{\theta}}'X, \sigma+X^{T}\Sigma_{\hat{\theta}}'X)$$


## Further Reading

<a href="https://en.wikipedia.org/wiki/Bayesian_linear_regression">Bayesian linear regression | Wikipedia</a>

<a href="https://cedar.buffalo.edu/~srihari/CSE574/Chap3/3.4-BayesianRegression.pdf">Bayesian linear regression | CEDAR, University at Buffalo, NY</a>

<a href="https://www.cs.utah.edu/~fletcher/cs6957/lectures/BayesianLinearRegression.pdf">Notes on Bayesian linear regression | School of Computing, University of Utah</a>

<a href="http://www.cs.cmu.edu/~16831-f14/notes/F14/16831_lecture20_jhua_dkambam.pdf">Bayesian linear regression | Statistical techniques in robotics, Carnegie Mellon University</a>