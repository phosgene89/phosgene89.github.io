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
To help tackle non-stationary data, we introduce an integration operator $\nabla^{d}$, defined as follows

$$ y_{t}^{[1]} =\nabla y_{t} = y_{t} - y_{t-1} $$

$$ y_{t}^{[2]} =\nabla^{2} y_{t} = y_{t}^{[1]} - y_{t-1}^{[1]} $$

$$ y_{t}^{[3]} =\nabla^{3} y_{t} = y_{t}^{[2]} - y_{t-1}^{[2]} $$

etc. In general

$$ y_{t}^{[d]} =\nabla^{d} y_{t} = y_{t}^{[d-1]} - y_{t-1}^{[d-1]} $$

We can now fit an $ARMA(p, q)$ model to $y_{t}^{[d]}$ rather than $y_{t}$. 

$$ y_{t}^{[d]} = \Theta(L)^{p} y_{t}^{[d]} + \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

This is equivalent to an $ARIMA(p,d,q)$ model on $y_{t}$

$$ y_{t} = \Theta(L)^{p} \nabla^{d} y_{t} + \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

## SARIMA
SARIMA models take seasonality into account by essentially applying an ARIMA model to lags that are integer multiples of seasonality. Once the seasonality is modelled, an ARIMA model is applied to the leftover to capture non-seasonal structure.

To see this more clearly, suppose we have a time series $ \{ y_{t} \} $ with seasonality s. We can try to eliminate the seasonality with differencing, by applying the differencing operator $\nabla_{s}^{D}$ to take the seasonal differences of the time series. 

$$ z_{t} = \nabla_{s}^{D} y_{t} $$

We can then capture any remaining structure by applying an ARMA(P, Q) model to $z_{t}$, but using seasonal lags. i.e. instead of using a regular lag operator $L$, we use $L^{s}$. 

$$ z_{t} = \theta(L^{s})^{P} z_{t} + \phi(L^{s})^{Q} \epsilon_{t} + \epsilon_{t} $$

$$ z_{t} = \theta(L^{s})^{P} \nabla_{s}^{D} y_{t} + \phi(L^{s})^{Q} \epsilon_{t} + \epsilon_{t} $$

With any seasonality now removed, we can apply another ARIMA(p, d, q) model to the resulting series.

$$ x_{t} = \Theta(L)^{p} \nabla^{d} z_{t} + \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

$$ x_{t} = \Theta(L)^{p} \nabla^{d} (\theta(L^{s})^{P} \nabla_{s}^{D} y_{t} + \phi(L^{s})^{Q}) + \Phi(L)^{q} \epsilon_{t} + \epsilon_{t}$$

## References

[1] Y. A. Malkov & D. A. Yashunin, <i>Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs</i> (2018), https://arxiv.org/abs/1603.09320

[2] D. J. Watts & S. H. Strogatz, <i>Collective dynamics of 'small-world’ networks</i> (1998), http://worrydream.com/refs/Watts-CollectiveDynamicsOfSmallWorldNetworks.pdf

[3] S. Milgran, The Small-World Problem (1967), http://snap.stanford.edu/class/cs224w-readings/milgram67smallworld.pdf

