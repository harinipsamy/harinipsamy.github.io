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
<img src="https://render.githubusercontent.com/render/math?math=E(r) = \sum_{i=1}^{n} p(i)r(i)">
variance = SD ^2

### Portfolio Mean

Mean of the portfolio returns is the weighted average of the sum of returns of the individual assets calculated as above.

<img src="https://render.githubusercontent.com/render/math?math=rp(i) = (x_AE(r_A)) ++ (x_BE(r_B))">, where x is the weight of each asset. 


### Portfolio risk

As long as the stocks in the portfolio are not perfectly correlated, the portfolio risk will be less than the weighted sum of individual risks.

<img src="https://render.githubusercontent.com/render/math?math=r_A^2 \sigma_A^2 \+ r_B^2 \sigma_B^2 \+ 2x_Ax_BCov(r_A,r_B)">
`r_A^2\sigma_A^2 + r_B^2 \sigma_B^2 + 2x_Ax_BCov(r_A,r_B)`
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
