---
title: "Statistics in quantitative trading"
date: 2020-04-29T15:34:30-04:00
categories:
  - blog
tags:
  - plan
  - finance
  - statistics
image: .jpg
---

<p>This includes a list of sub-topics to be covered in detail in separate posts:</p>
<!-- wp:list -->
<ul><li>Checking if the data is normally distributed</li><li>Stationarity</li><li>Transforming data to normally distributed stationary dataset</li><li>Importance of Linear regression in trading strategies</li></ul>
<!-- /wp:list -->
<p></p>

# Checking if data fits normal distribution

many tests assumes normal distribution of data.

If we say a data is normally distr
box plot to check for normality.
quantiles and quartiles, whiskers, outliers, interquartile range

hypothesis tests for normality
Shapiro-Wilk
D'Agostino-Pearson

hypothesis test for similarity of distri
Kolmogorov-Smirnov 

why care abt normality?
we use hypothesis test to test the stat models built. these tests assume data is normal. if data isn't normal, these tests might tell us that the model is valid when in fact, it isn't


stock returns - left skewed and fat tailed - extreme negative returns happen in more frequency

QQ plot (quantile quantile plot) quantiles have same number of data points (as against a histogram which has same width

## transform not-normal data
feed data into log function or take log of data

# Checking if data is stationary over time

stationarity - mean, variance and covariance are same over time. constant variance - homoscedastic, varying variance - heteroscedastic. Use Breusch-Pagan Test to test heteroscedasticity.

## transform heteroscedastic data

take time-difference. either take difference or take log returns. 

to make data normal & homoscedastic, use box-cox tranformation. 

# Regression

Independent variable - stock returns
dependent variable - unemployment rate, interest rates, consumer spending reports

y = ax + b
slope or coefficient = a, intercept = b

OLS - finds line that fits data with minimum distance.
residuals (actual - predicted) should be normally distributed
## evaluating model
- R-squared
- adjusted Rsqured
- F-test

multiple regression and multivariate multiple regression


using reg. to predict stoc returns is difficulst coz of lwo signal-noise ratio. also, models are sensitive to design choices in the model. 

still, reg is imp. coz - time series analysis, NNs are a bunch of regression blocks


## statistical arbitrage

We can also have stock returns as independent and dependent variables. predict stock returns of one stock by using stock returns of another stock.
simulatneously buying and selling related stocks based on their relationship.

signal to noise ration of financial data is very low. predictive models tend to overfit data.









