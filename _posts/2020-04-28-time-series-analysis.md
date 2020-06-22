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

## Stationarity

A time series is said to be stationary if the joint probability distribution does not change under time shift. For example, a time series `latex r_t` is stationary if the joint distribution of (r_t1,r_t2,...r_tn) is same as the joint probability distribution of say (r_t1+1, r_t2+1,...rtn+1). 

weak-stationarity: when both mean and the autocovariance of the time series are time invariant. In other words, we can say a time-series is weakly stationary if its first moment (mean) and second moment (variance) are constant. Stock return series are weakly stationary. If the time series is normally distributed, then weak stationarity is equivalent to strict stationarity.

A time series may be non-stationary due either to the presence of a unit root or of a deterministic trend. 


### trend-stationarity
When a time series is non-stationary due to a deterministic trend. Can be transformed to stationary process by differencing or removing seaonality
### unit root
The stochastic process with a unit root are not mean-reverting.

## Correlation and Autocorrelation

Autocorrelation function (ACF): 
that is they have a constant mean and variance.that is they have a constant mean and variance.

stock prices are non-stationary - mean and SD of data changes over time. goal of time-series analysis is to predict future values based on past values.

to make stock data more normal and stationary - use log returns 

# AR

tries to fit a line on previous values. 

yt = a +B1 yt-1 + B2yt-2+..+Et

we define AR model based on its lag. the no. of past values used in the AR model - lag
if we use 1 previous value to predict, the model is called AR lag 1 model. if we use 2 previous values to predict the current value and ignore the older values, it's called AR lag 2 model.

if coefficients are likely zero,we can remove that from the equation. 

## vector autoregressive model.

# MA

# ARMA

# ARIMA

# ML methods - Kalman & particle filters

particle filters do not assume normality and can work with non-linear data

# ML methods - RNN

used for NLP and time series. output of one regression is fed into input of another regression
