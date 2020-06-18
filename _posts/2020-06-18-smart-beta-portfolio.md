---
title: "Building a Smart Beta Portfolio"
tags:
  - portfolio management
  - portfolio construction
  - quantitative finance
---

Smart Beta portfolios apply rules-based weight of stocks in an index, rather than market cap as the weight.

## Identify Smart Beta strategy

Smart Beta Portfolios may target momentum,earnings quality, low volatility, dividends or some combination of them. 

## Calculate portfolio weights

Weights should be calculated based on the strategy chosen. Suppose the Smart Beta portfolio chooses weights based on dividend, then the weights of the stocks in the portfolio will be  proportionate to the dividend.

## Calculate Returns

Calculate dividend weighted returns of the stocks in the index. And then calculate cumulative returns.

## Compare performance

Compare performance using tracking error. 
Annualized tracking error, TE =\sqrt{252}*Sample_std_deviation(r_p-r_b), r_p=portfolio returns; r_b = benchmark returns

