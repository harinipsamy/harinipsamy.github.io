---
title: 'Standardazing data'
tags:
  - Data Science
  - Analysis
  - Machine Learning
  
---

Standardizing data is a very import step in feature engineering. This is also crucial in factor modeling. In this post, we look at how and why it is done.



some ways to standardize datasets
De-meaning:
Transform the data such that the mean of the dataset is zero. This process is called de-meaning, as we subtract (or remove) the sample mean from each observation.
Normalization:
2) Scaling- normalize the range of values of features. It allows for comparison between two variables which otherwise may have different range of values, to avoid giving too much importance to features with large values.  


## Standardizing a factor in factor model

1. Sum of weights to sum to 0 -> find mean of factor values of all the stocks. Then subtract mean from each of the factor values (de-mean)
2. Sum of absolute value of weights sum to 1 -> rescale val: divide each value by a scalar. this scalar is sum of the absolute values. There are different methods to scale features, based on the required purpose:
1. min-max normalization
2. Mean normalization
3. standardization
4. Rescaling with absolute value

can be done in any order

### why?
1. de-meaning
factor values will be used as stock weights. we want the portfolio to be dollar neutral ($ long = $ short). A $ neutral port., is approx market neutral. we assume that On avg., beta of each stock = 1. (beta = how much stock moves when the market moves)

Notional/trade book value = $ amt associated with port. : $ val. of inv. in a stock = wt. of stock in the port * notional

In theoritical $ neutral port, inv = $0. but in real life, there are margin cost for shorting positions, transac cost, etc

converting port to $ neutral port.:

port: A = 0.6 B = 0.4 -> this is heavier on the positive side
$ neutral port: shift the stock wts until the wts in positive side and negative side are balanced. -> A=0.1 B = -0.1

the relative difference of the wts of stocks in both port will be same: 0.6-0.4 = 0.2 and 0.1-(-0.1) = 0.2

This is achieved by de-meaning ( taking average of the wts and subtracting the avg from each of the wts)

2. rescaling
to make the leverage ratio =1 for our port.

leverage: borrowing to invest -> either by borrowing cash or taking short positions





