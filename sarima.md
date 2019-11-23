# <center>A Primer on SARIMAX</center>

## Overview

Seasonal Autoregressive Integrated Moving Average with Exogenous variable (SARIMAX) models are an extension of Autoregressive Integrated Moving Average with Exogenous variable (ARIMAX) models that are designed to take seasonality into account.

As SARIMAX is a generalization of simpler time series models, we first go through more specific cases in order of complexity. These are AR, MA, ARMA, ARIMA and SARIMA.

Check out the [statsmodels](https://www.statsmodels.org/stable/index.html) package to start using SARIMA in Python.

## Precursors to SARIMAX

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
To help tackle non-stationary data, we introduce an integration operator $\Delta^{d}$, defined as follows

$$ y_{t}^{[1]} =\Delta y_{t} = y_{t} - y_{t-1} $$

$$ y_{t}^{[2]} =\Delta^{2} y_{t} = y_{t}^{[1]} - y_{t-1}^{[1]} $$

$$ y_{t}^{[3]} =\Delta^{3} y_{t} = y_{t}^{[2]} - y_{t-1}^{[2]} $$

etc. In general

$$ y_{t}^{[d]} =\Delta^{d} y_{t} = y_{t}^{[d-1]} - y_{t-1}^{[d-1]} $$

We can now fit an $ARMA(p, q)$ model to $y_{t}^{[d]}$ rather than $y_{t}$. 

$$ y_{t}^{[d]} = \Theta(L)^{p} y_{t}^{[d]} + \Phi(L)^{q} \epsilon_{t}^{[d]} + \epsilon_{t}^{[d]}$$

This is equivalent to an $ARIMA(p,d,q)$ model on $y_{t}$

$$ \Delta^{d} y_{t} = \Theta(L)^{p} \Delta^{d} y_{t} + \Phi(L)^{q} \Delta^{d} \epsilon_{t} + \Delta^{d} \epsilon_{t}$$

With some algebra, we can re-arrange the equation and absorb constants into the polynomials $\Theta$ and $\Phi$. 

$$ \Theta(L)^{p} \Delta^{d} y_{t} = \Phi(L)^{q} \Delta^{d} \epsilon_{t}$$

## SARIMA
SARIMA models take seasonality into account by essentially applying an ARIMA model to lags that are integer multiples of seasonality. Once the seasonality is modelled, an ARIMA model is applied to the leftover to capture non-seasonal structure.

To see this more clearly, suppose we have a time series $ \{ y_{t} \} $ with seasonality s. We can try to eliminate the seasonality with differencing, by applying the differencing operator $\Delta_{s}^{D}$ to take the seasonal differences of the time series. 

We can then capture any remaining structure by applying an ARMA(P, Q) model to the differenced values, but using seasonal lags. i.e. instead of using a regular lag operator $L$, we use $L^{s}$. 

$$ \Delta_{s}^{D} y_{t} = \theta(L^{s})^{P} \Delta_{s}^{D} y_{t} + \phi(L^{s})^{Q} \Delta_{s}^{D} \epsilon_{t} + \Delta_{s}^{D} \epsilon_{t} $$

As with ARIMA, massaging the equation and absorbing constants into polynomials yields the following concise form

$$ \theta(L^{s})^{P} \Delta_{s}^{D} y_{t} =  \phi(L^{s})^{Q} \Delta_{s}^{D} \epsilon_{t} $$

With any seasonality now removed, we can apply another ARIMA(p, d, q) model to $ \Delta_{s}^{D} y_{t} $ by multiplying the seasonal model by the new ARIMA model.

$$ \Theta(L)^{p} \theta(L^{s})^{P} \Delta^{d} \Delta_{s}^{D} y_{t} = \Phi(L)^{q} \phi(L^{s})^{Q} \Delta^{d} \Delta_{s}^{D} \epsilon_{t}$$

This is the general form of a SARIMA(p, d, q)(P, D, Q, s) model.

## ARIMAX and SARIMAX

ARIMAX and SARIMAX models simply take exogenous variables into account - ie variables measured at time $t$ that influences the value of our time series at time $t$, but that are not autoregressed on. To do this, we simply add the terms in on the right hand side of our ARIMA and SARIMA equations.

For $n$ exogenous variables defined at each time step $t$, denoted by  $x_{t}^{i}$ for $ i \leq n $, with coefficients $\beta_{i}$, the ARIMAX(p, d, q) model is defined by

$$ \Theta(L)^{p} \Del^{d} y_{t} = \Phi(L)^{q} \Delta^{d} \epsilon_{t} + \sum_{i=1}^{n} \beta_{i} x^{i}_{t}$$

and the SARIMAX model by

$$ \Theta(L)^{p} \theta(L^{s})^{P} \Delta^{d} \Delta_{s}^{D} y_{t} = \Phi(L)^{q} \phi(L^{s})^{Q} \Delta^{d} \Delta_{s}^{D} \epsilon_{t} + \sum_{i=1}^{n} \beta_{i} x^{i}_{t} $$

## Further References

Chatfield, C 2004, The Analysis of Time Series : An Introduction, 6th ed., Chapman & Hall/CRC, Boca Raton, Fla.
