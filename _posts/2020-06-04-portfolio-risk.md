---
title: "Portfolio risk"
tags:
  - quantitative finance
  - stocks
  - algorithmic trading
---

When making investment decisions, it is as important to consider risk of the asset(s) and the asset classes as it is to consider the potential returns.

### Mean and variance 

Mean or average log return of a stock = expected value = sum of returns over period i/number of observations = sum_ prob. of observations x observations
<img src="https://latex.codecogs.com/svg.latex?E(r) = \sum_{i=1}^{n} p(i)r(i)">
<img src="https://latex.codecogs.com/svg.latex?variance = SD^2">

### Portfolio Mean

Mean of the portfolio returns is the weighted average of the sum of returns of the individual assets calculated as above.

<img src="https://latex.codecogs.com/svg.latex?rp(i) = x_AE(r_A) + x_BE(r_B)">, where x is the weight of each asset. 


### Portfolio risk

As long as the stocks in the portfolio are not perfectly correlated, the portfolio risk will be less than the weighted sum of individual risks.

<img src="https://latex.codecogs.com/svg.latex?r_A^2 \sigma_A^2 + r_B^2 \sigma_B^2 + 2x_Ax_BCov(r_A,r_B)">

Variance of 3-asset portfolio:


 


#### Quadratic form

#### Matrix form
The above formula can be written in matrix form as:


<img src="https://latex.codecogs.com/svg.latex?\sigma_P^2 = \begin {bmatrix} x_A & x_B\end {bmatrix}\begin {bmatrix} Cov(r_A,r_A) & Cov(r_A,r_B) \\ Cov(r_B,r_A) & Cov(r_B,r_B)\end {bmatrix}\begin {bmatrix} x_A \\ x_B\end {bmatrix}">


Let `P` be the covariance matrix:


<img src="https://latex.codecogs.com/svg.latex?P=\begin {bmatrix} Cov(r_A,r_A) & Cov(r_A,r_B) \\ Cov(r_B,r_A) & Cov(r_B,r_B)\end {bmatrix}">


and `x` be the weight vectors:


<img src="https://latex.codecogs.com/svg.latex?x = \begin {bmatrix} x_A \\ x_B\end {bmatrix}">


then,


<img src="https://render.githubusercontent.com/render/math?math=\sigma_P^2 =  x^TPx">



### Covariance matrix calculation
equal probability and mean is 0

### Other measures of risk:

#### Sharpe ratio

Sharpe ratio is the reward to risk ratio and is calculated as the excess of asset return over benchmark return, divided by the standard deviation of the excess asset return

<img src="https://latex.codecogs.com/svg.latex?excess\ return, a_t = r_{portfolio}%20-%20r_{rf,t}">

<img src="https://latex.codecogs.com/svg.latex?sharpe\ ratio = \dfrac%20{\dfrac{1}{T}%20\sum_{t=1}^{T}a_t}{\sqrt{\dfrac{\sum_{t=1}^{T}(a_t%20-%20\mu_{a,t})}{T-1}}">

<img src="https://latex.codecogs.com/svg.latex?annualized\ sharpe\ ratio = \sqrt{252} * sharpe\ ratio">


#### Downside risk

<img src="https://latex.codecogs.com/svg.latex?semi-deviation = \sum_{i=1}^{n} (\mu - r_i)^2 * I_{r_i < \mu}">

where <img src="https://latex.codecogs.com/svg.latex?\ I_{r_i < \mu} = 1\ if\ r_i<\mu\ and\ 0\ otherwise">
         



#### Value-at-Risk


## Limitations of mean and variance
