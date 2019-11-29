
# <center>Introduction to Bayesian Inference</center>

## Overview
Bayesian inference is based on predicting probability distributions for model parameters using Bayes' theorem

$$P(A|B) = \frac{P(B|A) P(A)}{P(B)} $$

## Distribution over point estimates
Suppose we want to estimate the parameters of a predictive model $y = f(x;\theta)$ paramaterised by $\theta$. The semi-colon notation inside $f(x;\theta)$ indicates that $\theta$ is fixed during our evaluation of $f$. We can use Bayes' theorem in conjunction with observed data $X, Y$ to calculate a probability distribution over $\theta$

$$p(\theta|Y;X) = \frac{p( Y| \theta; X) p(\theta)}{p(Y;X)}$$

Where 
$X$
is a matrix of covariates, 
$Y$
is a matrix of the target values and 
$\beta$ 
is a model parameter. 
$p(\theta)$ 
is called the *prior* distribution of 
$\theta$ 
and 
$p(\theta|Y;X)$ 
is called the *posterior* distribution of 
$\theta$.

## Prior Distribution
The prior distribution of $\theta$, $p(\theta)$, represents our prior knowledge of the distribution of $\theta$. Where we have no knowledge, we typically choose a prior 
distribution to be as broad as possible.

## Posterior Distribution
A posterior distribution of $\theta$, $p(\theta|Y;X)$, represents our knowledge of $\theta$ after having observed the data $X, Y$. Bayes' theorem provides a way for us to use data to calculate the posterior, given that we have the prior.

## Prediction
Given a distribution over $\theta$ and any given value for $x$, we can calculate a distribution over the values of $f(x;\theta)$ as follows

$$p(y|x) = \int_{\theta}p(y|\theta;x)p(\theta)d\theta $$

In other words, we are taking a weighted average over all possible paramaterisations of $f(x;\theta)$.

## Maximum a Posteriori Estimation
The Bayesian counterpart of maximum likelihood estimation is maximum a posteriori estimation (MAP). The difference is that we incorporate 
the prior into our calculation of the likelihood function. Suppose we want a MAP estimation of $\theta$, then the MAP estimation is obtained from

$$ \theta_{MAP} = argmax_{\theta}(L(Y | \theta;X) p(\theta)) $$

Where $L$ is the likelihood function and $p(\theta)$ is the prior for $\theta$.

## General Procedure

### 1. Choose model paramaterisation.
Select a paramaterisation for $f(x;\theta)$. e.g. for linear regression forced to intersect the origin, we choose $f(x;\theta)=\theta x$

### 2. Choose priors.
Choose an initial distribution for $\theta$. A common choice is a Gaussian distribution $p(\theta) = \frac{1}{\sqrt{2\pi\sigma^{2}}}exp(-\frac{(\theta - \mu)^2}{2\sigma^{2}})$ with mean $\mu$ and standard deviation $\sigma$.

### 3. Gather data and use Bayes' theorem to calculate posterior probabilities.
Use the following update equation (this is where a lot of the difficulty with Bayesian inference shows itself)

$$p(\theta|Y;X) = \frac{p( Y| \theta; X) p(\theta)}{p(Y;X)}$$

$$p(\theta|Y;X) = \frac{p( Y| \theta; X) p(\theta)}{\int_{\theta}p(Y|\theta;X)p(\theta)d\theta}$$

### 4. Repeat 3 as needed.
For further updates, we set the posterior of step 3. as our new prior and repeat step 3. with new data.

## Why use Bayesian inference?

### More intuitive
The frequentist hypothesis testing framework is difficult to interpret - p-values in particular are notorious for being misused. Bayesian methods, in comparison, are very intuitive. 

For example, a 95% confidence interval over $\theta$ is some given range over which, if we re-sampled the data, re-did the experiment and recalculated confidence interval, it would contain the actual value of $\theta$ in 95% of these imaginary repeat experiments.

Bayesian methods methods simply give a probability distribution of $\theta$ which can be used to build a Bayesian confidence that has a 0.95 probability of containing $\theta$. Much simpler.

### Easy to include prior knowledge
Bayesian priors allow us to easily use our pre-existing knowledge of a problem to improve our modelling by simply adjusting our prior. e.g. if know from previous experimentation and experience that the parameter we are trying to estimate, $\theta$, is distributed around 100 and rarely moves below 95 and above 105, we can model our prior as a normal distribution centred at 100 and with a standard deviation of ~2.

## Why *not* to use Bayesian inference?

### Computationally intensive
The integral in the denominator of the right hand side of the Bayesian update rule is in general difficult (or even impossible) to evaluate analytically and must be solved with approximate numerical methods (Markov Chain Monte Carlo - MCMC). These methods are computationally intense compared to frequentist methods of inference.

### Priors
There is no clear-cut procedure for choosing priors. Oftentimes prior distributions are chosen simply because they are mathematically convenient. Other times they can be biased by personal preferences - which seems very scientifically distasteful. Exactly what makes a good prior is not established, which can be an issue for some.

## Further Reading

Bishop, C.M., 2006. *Pattern recognition and machine learning*. Springer Science+ Business Media.

MacKay, D.J. and Mac Kay, D.J., 2003. *Information theory, inference and learning algorithms*. Cambridge university press.

Gelman, A., Carlin, J.B., Stern, H.S., Dunson, D.B., Vehtari, A. and Rubin, D.B., 2013. *Bayesian data analysis*. Chapman and Hall/CRC.