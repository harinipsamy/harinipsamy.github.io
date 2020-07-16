---
title: "Time series analysis for quantitative trading"
tags:
  - blog
  - statistics
  - quantitative finance
  - AI in investment
  - finance
---

Overview of time series analysis topics covered in this post:

AutoRegression

MovingAverages

ARCH and GARCH models

Kalman filters

Particle filters

Recurrent Neural Networks


# Concepts relevant to time series analysis

## Moments of a Random Variable

The first moment is the mean. The second moment is its variance. The first and second moments of a random variable uniquely determine a normal distribution. For other distributions, higher order moments are also relevant.

The third moment measures the symmetry of the random variable with respect to its mean, and is referred to as skewness. The fourth moment measures the tail behavior of the random variable and is called as the kurtosis. Kurtosis is 3 for a normal distribution. A distribution with positive excess kurtosis (kurtosis greater than 3) has heavy tails and is leptokurtic. It means that it has more extreme values. A distribution with negative excess kurtosis has short tails and is said to be platykurtic.

## Stationarity

A time series is said to be stationary if the joint probability distribution does not change under time shift. For example, a time series <img src="https://latex.codecogs.com/svg.latex?r_t"> r<sub>t</sub> `r<sub>t</sub>` is stationary if the joint distribution of (r_t1,r_t2,...r_tn) is same as the joint probability distribution of say (r_t1+1, r_t2+1,...rtn+1). 

weak-stationarity: when both mean and the autocovariance of the time series are time invariant. In other words, we can say a time-series is weakly stationary if its first moment (mean) and second moment (variance) are constant. Stock return series are weakly stationary. If the time series is normally distributed, then weak stationarity is equivalent to strict stationarity.

For a stationary series, the multistep-ahead forecasts converge to the mean of the series and the variance of forecast errors converge to the variance of the series, as the forecast period increases. 
A time series may be non-stationary due either to the presence of a unit root or of a deterministic trend. 


#### Trend-stationarity

When a time series is non-stationary due to a deterministic trend. Can be transformed to stationary process by differencing or removing seaonality

#### Unit root

The stochastic process with a unit root are not mean-reverting.

## Correlation and Autocorrelation

Correlation coefficient between two random variables X and Y:

<img src="https://latex.codecogs.com/svg.latex?\rho_{x,y} = \frac{Cov(X,Y)}{\sqrt{Var(X)Var(Y)}}">

Autocorrelation function (ACF): 
The correlation coefficient between <img src="https://latex.codecogs.com/svg.latex?r_t"> and <img src="https://latex.codecogs.com/svg.latex?r_{t-n}"> is called the lag-n autocorrelation of <img src="https://latex.codecogs.com/svg.latex?r_{t}">, where <img src="https://latex.codecogs.com/svg.latex?r_{n}"> is a weakly stationary return series. It refers to the correlations between a variable and its past values.

<img src="https://latex.codecogs.com/svg.latex?\rho_{n} = \frac{Cov(r_t,r_{t-n})}{Var(r_t)}">

A linear time series model can be characterized by its ACF.

The linear dependence of current return r_t on the remote past return r_t-n diminishes for large n.

Under the efficient market assumption, there should be zero autocorrelations as the asset returns are not predictable.

that is they have a constant mean and variance.that is they have a constant mean and variance.


stock prices are non-stationary - mean and SD of data changes over time. goal of time-series analysis is to predict future values based on past values.

to make stock data more normal and stationary - use log returns 

# AR models

### What it is

tries to fit a line on previous values. 

yt = a +B1 yt-1 + B2yt-2+..+Et

we define AR model based on its lag. the no. of past values used in the AR model - lag
if we use 1 previous value to predict, the model is called AR lag 1 model. if we use 2 previous values to predict the current value and ignore the older values, it's called AR lag 2 model.

if coefficients are likely zero,we can remove that from the equation. 

AR(1) model is similar to simple linear regression model in which r_t is the dependent variable and r_t-1 is the explanatory variable:
r_t = phi_0 + phi_1 r_t-1 + a_t

A generalized AR(p) model is in the same form as a multiple linear regression model with lagged values as the explanatory variables:
r_t = phi_0 + phi_1 r_t-1 +... + phi_p r_t-p + a_t

a_t is white noise.


### Properties
The ACF of a weakly stationary AR(1) series decays exponentially with rate phi_1.
For an AR series to be stationary, all of its characteristic roots must be less than |1|
Stationarity:
### Identifying order
Two methods: 
1) PACF: PACF of a stationary time series is a function of its ACF. For an AR(p) model, the lag p sample should not be zero but should be zero for all n>p. In other words,for an AR(p) series, the sample PACF cuts off at lag p.
2) Information criterion:AIC and BIC are two measures based on likelihood to determine the order of p of an AR series. The smaller the value, the better and usually the smallest AIC/BIC values may be considered to identify p. AIC and BIC may identify different lags as p, in which case we must use our judgement to determine the order.

#### Model checking
A fitted model must be checked for inadequacies. If the model is adequate, then the residual series will be white noise. The ACF and Ljung-Box test may be used on the residuals to check if its close to white noise.  

### Goodness of Fit
goodness of fit of a stationary model is measured by R square (R^2) defined as:
R^2 = 1- residual sum of squares/total sum of squares.
Typically, a larger R^2 indicates closer fit to data. But this is only true for stationary time series. R^2 is a nondecreasing function of the number of parameters used. To overcome this, an adjusted R^2 is used, but the values don't lie between 0 and 1:
Adj R^2 = 1 - variance of residuals/variance of r_t

### Forecasting


## vector autoregressive model.

# Moving-Average models
MA(q) model:
r_t = c_0 + a_t - Theta_1 a_t-1 - theta_2 a_t-2- ...- theta_q a_t-q
c_0 is a constant. {a_t} is white noise series

### Properties

Moving-average models are always weakly stationary. 
ACF of MA(q) model cuts off at lag q. 

### Identifying order
ACF is used to determine the order of MA model. For a MA(q) model, the ACF at q notequalto 0, but is equal to 0 for all l> q.
Unlike the sample PACF, sample ACF provides information on the nonzero MA lags of the model.



# Autoregressive Moving-Average (ARMA) models

ARMA models combine AR and MA models. It is relevant in volatility modeling. 

ARMA(1,1) model:

r_t - phi_1 r_t-1 = phi_0 + a_t - phi_1 a_t-1

ARMA(p,q) model:

r_t = phi_0 + sum{i=1}{p} phi_i r_t-i + a_t - sum{i=1}{q} phi_i a_t-i

{a_t} is white noise. 


ACF and PACF are not useful in determining the order of an ARMA model. Instead, Extended Auto-correlation function (EACF) is used. It is a table with both AR and MA and for an ARMA(p,q) model, the upper left start of the triangle of Os will be the (p,q) position. 

# Unit-root nonstationarity

Interest rates, forex rates, price series of an asset/product tend to be nonstationary. Random walk model is another example of unit-root nonstationary time series.


# ML methods - Kalman & particle filters

particle filters do not assume normality and can work with non-linear data

# ML methods - RNN

used for NLP and time series. output of one regression is fed into input of another regression
