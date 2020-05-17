---
title: "Time series analysis for quantitative trading"
date: 2020-05-15
categories:
  - blog
tags:
  - finance
  - statistics
#image: .jpg
---

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
