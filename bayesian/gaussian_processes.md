# <center>Gaussian Processes</center>

## Overview
A Gaussian proccess is a generalization of a multivariable Gaussian distribution to infinitely many dimensions. ie we are going from discrete to continuous. A Gaussian process is fully defined by a mean function and covariance function.

Formally, a Gaussian process is defined as a process that generates data throughout some domain such that any finite subset of the range is Gaussian.

Regression with Gaussian processes enables us to calculate the uncertainty over our predictions by integrating over all potential parameterised models. While this may be computationally intractable for most distributions, Gaussian distributions are (relatively) easy to integrate. Due to Gaussians being conjugate priors of themselves, after updating our priors we are still left with nice, well behaved Gaussians.

## Gaussian Processes for Regression
As in traditional linear regression, we assume that our residuals are normally distributed with a mean of 0. 

Instead of forming a distribution over the parameters of a single model, we form a distribution over an infinite amount of models built from Gaussian processes. 

$$ y_{i} = f(\textbf{x}) + \epsilon$$

$$ f \sim GP(.|0,K) $$

$$\epsilon \sim \mathcal{N}(.|0, \sigma^{2}) $$

where $\epsilon$ is white noise, $K$ is the covariance function and $\sigma$ is the residual standard deviation (assumed to be constant).

We then make predictions by integrating over $f$

$$p(y_{*}|\textbf{x}_{*}, X, Y) = \int p(y| f;\textbf{x}_{*}, X, Y)p(f;X,Y)df$$

A common choice for the covariance function is the exponential square distance

$$k(x, x') = \sigma^{2}_{f} exp\left(-\frac{(x-x')^{2})}{2l^{2}}\right)$$

Where $\sigma^{2}_{f}$ is the maximum covariance and $l$ governs how quickly the correlation betwen points falls with the distance between them. More complex covariance functions can be required to model functions with greater complexity. The only requirement on our covariance function is that it be *positive definite*.

The noise associated with each measurement is incorporated into the covariance function as follows

$$k(x, x') = \sigma^{2}_{f} exp\left(-\frac{(x-x')^{2})}{2l^{2}}\right)+\sigma^{2}_{n}\delta(x,x')$$

Where $\sigma_{n}$ is the residual standard deviation and $\delta(x,x')$ is the Kroenecker delta function.

We can model $Y$ as follows

$$Y = \begin{vmatrix}
\textbf{y}\\
\textbf{y}^{*}\\
\end{vmatrix}
\sim
\mathcal{N}\left( \textbf{0}, 
\begin{vmatrix}
K&K_{*}^{T}\\
K_{*}&K_{**}\\
\end{vmatrix}
\right)$$

Where $K$ is the covariance matrix for $X$, $K_{*}$ is the covariance matrix for $X$ and $X^{*}$ and $K_{**}$ is the covariance matrix for $X^{*}$. 

# Further References
Best reference text: http://www.gaussianprocess.org/gpml/chapters/

https://katbailey.github.io/post/gaussian-processes-for-dummies/

http://mlg.eng.cam.ac.uk/teaching/4f13/1920/gaussian%20process.pdf

https://www.cs.toronto.edu/~hinton/csc2515/notes/gp_slides_fall08.pdf

https://mlss2011.comp.nus.edu.sg/uploads/Site/lect1gp.pdf

http://www.inference.org.uk/mackay/gpB.pdf

https://www.ritchievink.com/blog/2019/02/01/an-intuitive-introduction-to-gaussian-processes/

https://learning.mpi-sws.org/mlss2016/slides/gp_mlss16.pdf

https://arxiv.org/abs/1505.02965 Gaussian Processes: A Quick Introduction

https://github.com/jkfitzsimons/IPyNotebook_MachineLearning/blob/master/Just%20Another%20Kernel%20Cookbook....ipynb A bunch of different kernels for GP in Python.