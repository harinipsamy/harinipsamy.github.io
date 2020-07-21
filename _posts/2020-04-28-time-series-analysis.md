---
title: "Time series analysis for quantitative trading"
tags:
  - blog
  - statistics
  - quantitative finance
  - AI in investment
  - finance
---

Time Series Analysis is an important topic in quantitative analysis of stocks. The goal of time-series analysis is to predict future values based on the past. This post covers some basic concepts relevant to time series analysis.


## Moments of a Random Variable

The first moment is the mean. The second moment is its variance. The first and second moments of a random variable uniquely determine a normal distribution. For other distributions, higher order moments are also relevant.

The third moment measures the symmetry of the random variable with respect to its mean, and is referred to as skewness. The fourth moment measures the tail behavior of the random variable and is called as the kurtosis. Kurtosis is 3 for a normal distribution. A distribution with positive excess kurtosis (kurtosis greater than 3) has heavy tails and is leptokurtic. It means that it has more extreme values. A distribution with negative excess kurtosis has short tails and is said to be platykurtic.

## Stationarity

A time series is said to be stationary if the joint probability distribution does not change under time shift. For example, a time series <img src="https://latex.codecogs.com/svg.latex?r_t"> is stationary if the joint distribution of <img src="https://latex.codecogs.com/svg.latex?(r_{t_1},r_{t_2},...r_{t_n})"> is same as the joint probability distribution of say, <img src="https://latex.codecogs.com/svg.latex?(r_{t_{1+1}}, r_{t_{2+1}},...r_{t_{n+1}})">. 

Weak-stationarity is observed when both mean and the autocovariance of the time series are time invariant. In other words, we can say a time-series is weakly stationary if its first moment (mean) and second moment (variance) are constant. Stock return series are weakly stationary. If the time series is normally distributed, then weak stationarity is equivalent to strict stationarity.

For a stationary series, the multistep-ahead forecasts converge to the mean of the series and the variance of forecast errors converge to the variance of the series, as the forecast period increases. 

A time series may be non-stationary due either to the presence of a unit root or of a deterministic trend:

#### Trend-stationarity

When a time series is non-stationary due to a deterministic trend. It can be transformed to stationary process by differencing and/or removing seaonality.

#### Unit root

The stochastic process with a unit root are not mean-reverting. Interest rates, forex rates, price series of an asset/product tend to be nonstationary. Random walk model is another example of unit-root nonstationary time series.

## Correlation and Autocorrelation

Correlation coefficient between two random variables X and Y is defined as:

<img src="https://latex.codecogs.com/svg.latex?\rho_{x,y} = \frac{Cov(X,Y)}{\sqrt{Var(X)Var(Y)}}">

Autocorrelation function (ACF): 

The correlation coefficient between <img src="https://latex.codecogs.com/svg.latex?r_t"> and <img src="https://latex.codecogs.com/svg.latex?r_{t-n}"> is called the lag-n autocorrelation of <img src="https://latex.codecogs.com/svg.latex?r_{t}">, where <img src="https://latex.codecogs.com/svg.latex?r_{n}"> is a weakly stationary return series. It refers to the correlations between a variable and its past values.

<img src="https://latex.codecogs.com/svg.latex?\rho_{n} = \frac{Cov(r_t,r_{t-n})}{Var(r_t)}">

A linear time series model can be characterized by its ACF.

The linear dependence of current return <img src="https://latex.codecogs.com/svg.latex?r_{t}"> on the remote past return <img src="https://latex.codecogs.com/svg.latex?r_{t-n}"> diminishes for large n.

Under the efficient market assumption, there should be zero autocorrelations as the asset returns are not predictable.


# Auto-Regressive models

<img src="https://latex.codecogs.com/svg.latex?y_t = a +B_1 y_{t-1} + B_2 y_{t-2} +..+ E_t,\ where\ E_t\ is\ the\ error\ term.">

The model tries to fit a line on previous values. we define AR model based on its lag. the number of past values used in the AR model is its lag. If we use one previous value to predict the current value, the model is called AR lag-1 model (also represented as AR(1) model). if we use two previous values to predict the current value and ignore the older values, it's called AR lag-2 model.

AR(1) model is similar to simple linear regression model in which <img src="https://latex.codecogs.com/svg.latex?r_t"> is the dependent variable and <img src="https://latex.codecogs.com/svg.latex?r_{t-1}"> is the explanatory variable:

<img src="https://latex.codecogs.com/svg.latex?r_t = \phi_0 + \phi_1 r_{t-1} + a_t">


A generalized AR(p) model is in the same form of a multiple linear regression model with lagged values as the explanatory variables:

<img src="https://latex.codecogs.com/svg.latex?r+t = \phi_0 + \phi_1 r_{t-1} + ... + \phi_p r_{t-p) + a_t, where a_t is the white noise">


### Properties

The ACF of a weakly stationary AR(1) series decays exponentially with rate phi_1.
For an AR series to be stationary, all of its characteristic roots must be less than |1|

### Identifying order

There are two methods by which the order of an Auto-regressive model can be identified:

1) PACF: PACF of a stationary time series is a function of its ACF. For an AR(p) model, the lag p sample should not be zero but should be zero for all n>p. In other words,for an AR(p) series, the sample PACF cuts off at lag p.

2) Information criterion: AIC and BIC are two measures based on likelihood to determine the order of p of an AR series. The smaller the value, the better and usually, the smallest AIC/BIC values may be considered to identify p. Sometimes, AIC and BIC may identify different lags as p, in which case we must use our judgement to determine the order.

#### Model checking

A fitted model must be checked for inadequacies. If the model is adequate, then the residual series will be white noise. The ACF and Ljung-Box test may be used on the residuals to check if the distribution of residuals is close to white noise.  

### Goodness of Fit

goodness of fit of a stationary model is measured by R square (R^2) defined as:

<img src="https://latex.codecogs.com/svg.latex?R^2 = 1 - \frac{Residual sum of squares}{Total sum of sqares}">


Typically, a larger R<sup>2</sup> indicates closer fit to data. But this is only true for stationary time series. R^2 is a nondecreasing function of the number of parameters used. To overcome this, an adjusted-R<sup>2</sup> is used, but the values don't lie between 0 and 1:
  
<img src="https://latex.codecogs.com/svg.latex?Adjusted R^2 = 1 - \frac{Variance of residuals}{Variance of r_t}">



# Moving-Average models

MA(q) model:

<img src="https://latex.codecogs.com/svg.latex?r_t = c_0 + a_t - \theta_1 a_{t-1} - \theta_2 a_{t-2} - ... - \theta_q a_{t-q},\ where\ c_0\ is\ a\ constant\ and\ a_t\ is\ the\ white\ noise\ series">


### Properties

- Moving-average models are always weakly stationary. 
- ACF of MA(q) model cuts off at lag q. 

### Identifying order

ACF is used to determine the order of MA model. For a MA(q) model, the ACF at <img src="https://latex.codecogs.com/svg.latex?q \neq 0,\ but\ q = 0\ for\ all\ l>q"> 

Unlike the sample PACF, sample ACF provides information on the nonzero MA lags of the model.


# Autoregressive Moving-Average (ARMA) models

ARMA models combine AR and MA models. It is relevant in volatility modeling. 

ARMA(1,1) model:

<img src="https://latex.codecogs.com/svg.latex?r_t - \phi_1 r_{t-1} = \phi_0 + a_t - \phi_1 a_t-1">



ARMA(p,q) model:

<img src="https://latex.codecogs.com/svg.latex?r_t = \phi_0 + sum{i=1}{p} \phi_i r_t-i + a_t - sum{i=1} {q} \phi_i a_{t-i},\ where\ a_t\ is\ white\ noise">



ACF and PACF are not useful in determining the order of an ARMA model. Instead, Extended Auto-correlation function (EACF) is used. It is a table with both AR and MA and for an ARMA(p,q) model, the upper left start of the triangle of Os will be the (p,q) position. 


