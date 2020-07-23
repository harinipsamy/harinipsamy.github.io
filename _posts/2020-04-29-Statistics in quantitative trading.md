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

### Checking if data is normally distributed

We use various hypothesis tests to test the statistical models built. These tests assume that the distribution of data is normal. If data isn't normally distributed, these tests might tell us that the model is valid when in fact, it isn't. It is therefore, important to ensure that the data fits normal distribution.

We can check visually if the data follows a normal distribution by plotting its histogram and compare it with a plot of normal distribution. We can also use Box-Whisker plot to check for normality. Box plot helps to see symmetry in our data. Normal distributions are symmetric around their mean. The three dividing lines group the data into 4 and are called as Quartiles. The four groups are called as Quantiles. The second line represents the median. Inter Quartile Range can be calculated by 3rd Quartile - 1st Quartile. Points lying outside the whiskers are outliers. QQ plot (Quantile - Quantile plot) can also be used for checking normality. It checks if the shape of the distribution of data matches the shape of a distribution function. To decide if the data is normally distributed, we can compare the data distribution with distribution of a normal density function. 

There are also hypothesis tests to check for normality - Shapiro-Wilk test, D'Agostino-Pearson test and Kolmogorov-Smirnov test. If the p-value is less than alpha (say, 0.05), then we can say with 95% confidence that the data is NOT normally distributed. 

### Transforming data

To reshape the data to make it more normal, it is fed into a log function. In other words, we take log of data.

A sequence of random variables is homoskedastic if all its random variables have the same variance. Heteroskedasticity is when there is change in variance over time. Breusch-Pagan Test is used to test if data is heteroskedastic. if p-value is less than 0.05, then the data is heteroskedastic. 

To get a homoskedastic data, take time-difference. either take difference or take log returns. To make data both normal & homoscedastic, we use box-cox tranformation:

<img src="https://latex.codecogs.com/svg.latex?T(x) = \frac{(x^\lambda -1)}{\lambda}">

T(x) = \frac{(x^\lambda -1)}{\lambda}

<img src="https://latex.codecogs.com/svg.latex? \lambda"> is a constant we can choose. if <img src="https://latex.codecogs.com/svg.latex? \lambda"> is zero, then the transformation data is just the natural log:

<img src="https://latex.codecogs.com/svg.latex?T(x) = \ln(x)">
T(x) = ln(x)



