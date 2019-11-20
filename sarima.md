# <center>A Primer on SARIMA</center>

## Overview

Seasonal Autoregressive Integrated Moving Average (SARIMA) models are an extension of ARIMA models that are designed to take seasonality into account.

As SARIMA is a generalization of simpler time series models, we first go through more specific cases in order of complexity. These are AR, MA, ARMA and ARIMA

Check out the [statsmodels](https://www.statsmodels.org/stable/index.html) package to start using SARIMA in Python.

## Precursors to SARIMA

### Autoregressive (AR) Models
Suppose we have a time series given by $\{ y_{t} \}$. An $AR(p)$ model can be specified by

$$ y_{t} = constant + \epsilon_{t} + \sum\limits_{i=1}^p \theta_{i} y_{t-i} $$

Where $\epsilon_{t}$ is the noise at time t. 

This equation can be made more concise through the use of the lag operator, $L$.

$$L^{n} y_{t} = y_{t-n}$$

Taking $\Theta(L)^{p}$ to be an order p polynomial function of $L$, we can instead define an autoregressive model by

$$ y_{t} = \Theta(L)^{p} y_{t} + \epsilon_{t}$$

Taking note that the constant has been absorbed into the polynomial $\Theta$.

### Moving average (MA) Models.
Where autoregressive models regress on prior values of y_t, moving average models regress on prior values of error. An $MA(q)$ model can be specified by

$$ y_{t} = \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

Where $\Phi$ is defined analagously to to $\Theta$.

### Autoregressive Moving Average (ARMA) Models
$ARMA(p,q)$ models are simply a sum of $AR(p)$ and $MA(q)$ models.

$$ y_{t} = \Theta(L)^{p} y_{t} + \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

### Autoregressive Integrated Moving Average (ARIMA) Models
text


## SARIMA
text

## References

[1] Y. A. Malkov & D. A. Yashunin, <i>Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs</i> (2018), https://arxiv.org/abs/1603.09320

[2] D. J. Watts & S. H. Strogatz, <i>Collective dynamics of 'small-world’ networks</i> (1998), http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf

[3] S. Milgran, The Small-World Problem (1967), http://snap.stanford.edu/class/cs224w-readings/milgram67smallworld.pdf

