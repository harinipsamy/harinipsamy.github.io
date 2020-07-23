---
title: "Momentum Trading"

categories:
  - blog
tags:
  - building trading strategy
  - finance

---


Momentum based trading is based on the theory that rising stocks continue to rise and falling stock prices keep falling. Outperforming stocks will continue to outperform while underperforming stocks will continue to underperform in the short-term, other things being equal.
We can either take long or short position based on the momentum that we are tracking/want to take advantage of.

### Steps in Momentum trading

1. Choose a stock universe - for example, s&p 500 composition. Beware of survivorship bias
2. Re-sample daily prices to month-end prices and compute log returns
3. Rank by month-end returns
4. Compute portfolio returns (long - short)
5. Step 4 is theoritical monthly performance. Is the mean monthly return > 0? check by using t-test. The null hypothesis states that the true mean is 0. If p-value < 0.05 (or alpha), we reject the null hypothesis that true mean = 0, which means that we are fairly unlikely to get a positive return in step 4 if the true mean is indeed 0 and implies there might be some alpha in the strategy. Shows initial promise -> proceed to investigate further 
6. Fine-tune strategy and backtest.


