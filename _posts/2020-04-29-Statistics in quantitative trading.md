---
title: "Statistics in quantitative trading"
date: 2020-04-29T15:34:30-04:00
categories:
  - blog
tags:
 
  - finance
  - statistics
#image: .jpg
---

<p>This post touches upon topics such as:</p>
<!-- wp:list -->
<ul><li>Checking if the data is normally distributed</li><li>Stationarity</li><li>Transforming data to normally distributed stationary dataset</li><li>Importance of Linear regression in trading strategies</li></ul>
<!-- /wp:list -->
<p></p>

# Checking if data is normally distributed

We use various hypothesis tests to test the statistical models built. These tests assume that the distribution of data is normal. If data isn't normally distributed, these tests might tell us that the model is valid when in fact, it isn't. It is therefore, important to ensure that the data fits normal distribution.

We can roughly check visually if the data follows a normal distribution by plotting its histogram and compare it with a plot of normal distribution.

We can also use Box-Whisker plot to check for normality. Box plot helps to see symmetry in our data. Normal distributions are symmetric around their mean. The three dividing lines group the data into 4 and are called as Quartiles. The four groups are called as Quantiles. The second line represents the median. Inter Quartile Range can be calculated by 3rd Quartile - 1st Quartile. Points lying outside the whiskers are outliers.

QQ plot (Quantile - Quantile plot) can also be used for checking normality. It checks if the shape of the distribution of data matches the shape of a distribution function. To decide if the data is normally distributed, we can compare the data distribution with distribution of a normal density function. 


There are also hypothesis tests to check for normality - Shapiro-Wilk test, D'Agostino-Pearson test and Kolmogorov-Smirnov test. If the p-value is less than alpha (say, 0.05), then we can say with 95% confidence that the data is NOT normally distributed. 


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









