---
title: Momentum strategy based on historical performance - part 2

tags: 
 - quantitative research
 - algorithmic trading
 - stock market
author: Harini Palanisamy
---

This is continuation of the momentum strategy based on historical performance part-1, which can be found [**here**](https://harinipsamy.github.io/2020/05/20/momentum-strategy-1-part1/).

## Ranking stocks based on performance

### Converting data into monthly series

For each month-end observation period, I ranked the stocks by previous returns to find the best and worst performing stocks during the period. Ranking performance and taking positions on a daily basis may be cumbersome and costly. Rebalancing portfolio based on monthly performance seems like a good idea to start with. 

Pandas has resample method that converts the data into a different time factor. The data from quantquote is daily, but it can be converted to weekly, monthly or yearly using the resample method. One thing to note here is that the method requires the index of the dataframe to be a time-series. If the dates are not time-series type, they can be converted using .to_datetime() method, which was done at the beginning, while reading the data.

Since we are transforming closing prices, last instance of the period is relevant. To transform opening, low, and high prices, we would take .first(), .min() and .max() values respectively. 


```python
monthly = close.resample('M').last()
```

### Calculate returns

Finally, calculate monthly returns of the stocks and also calculate lookahead returns to compare performance...

```python
returns = np.log(monthly) - np.log(monthly.shift(1))
prev_returns = returns.shift(1)
lookahead_returns = returns.shift(-1)
```

### Rank stocks 

... and rank the stocks based on the monthly returns in the order of highest to lowest.

```python
# rank the stocks by prev returns from highest to lowest
def rank_(prev_returns, n):
    top_prices = pd.DataFrame(0, index=prev_returns.index, columns=prev_returns.columns)
    for index, col in prev_returns.iterrows():
        top_prices.loc[index, col.nlargest(n).index] = 1
    return top_prices

```

### Select the stocks to buy/sell

Select the top 10 performing stocks for the long position, and the 10 bottom performing stocks for the short position


```python
# select top for long, bottom for short
long = rank_(prev_returns, 10)
short = rank_(-1 * prev_returns, 10) 

```

## Calculating portfolio returns

Once the stocks for long and short positions are selected, it's time to check if buying/selling the stocks would actually result in a profit. We calculate portfolio returns for the positions taken:

```python
# calculate expected portfolio returns
port_ret = (long * lookahead_returns - short * lookahead_returns)/(10+10)
port_ret.iloc[:,:10].head()
```

```
ticker 	a 	aa 	aapl 	abbv 	abc 	abt 	ace 	acn 	act 	adbe
date 										
1998-01-31 	NaN 	0.0 	0.000000 	NaN 	0.0 	0.0 	0.0 	NaN 	0.0 	0.0
1998-02-28 	NaN 	0.0 	0.000000 	NaN 	0.0 	0.0 	0.0 	NaN 	0.0 	0.0
1998-03-31 	NaN 	0.0 	-0.000456 	NaN 	0.0 	0.0 	0.0 	NaN 	0.0 	0.0
1998-04-30 	NaN 	0.0 	0.000000 	NaN 	0.0 	0.0 	0.0 	NaN 	0.0 	0.0
1998-05-31 	NaN 	0.0 	0.000000 	NaN 	0.0 	0.0 	0.0 	NaN 	0.0 	0.0
```



### Average portfolio return

The overall average return generated by the portfolio is:

```python
(np.exp(port_ret.T.sum().mean() * 12 ) -1 ) * 100
```
```
7.575001799545111
```

Six to eight percent is the average return that an investor would get from investing in the stock market index in the long-term. By we are actively buying and selling curated stocks from the index universe that performed better in the past month and are expected to have momentum effect, the portfolio has performed similar to passively investing in the index.

### Negative returns

Let's check the number of instances of negative returns in the portfolio:

```python
(port_ret.T.sum() <0).astype(int).sum()
```

```
79
```
The portfolio's returns were negative 79 times out of 188 months. It is understandable that even a strategy based on pure momentum can perform negatively every now and then. 40% seems a little high, but we can compare this with the benchmark performance to conclude if this is good or bad. Another thing to compare with the benchmark would be the maximum drawdown, to assess the risk.

## statistical tests

Finally, let's test if the positive mean return from the portfolio is a one-off or it's likely to give positive returns with statistical significance.

T-test checks for null-hypothesis that the true mean is zero. Therefore, to imply that the portfolio's mean return is positive, the p-value should be less than the stated level of significance. I used alpha = 5% for this test.

```python
def t_test(returns):
    t_value = stats.ttest_1samp(returns,0.0)[0]
    p_value = stats.ttest_1samp(returns,0.0)[1]/2
    if p_value < 0.05:
        result = "Strategy might work. proceed further"
    else:
        result = "rethink strategy. There doesn't seem to be an alpha factor involved"
    return t_value, p_value, result

```

```python
t, p , result= t_test(port_ret.T.sum())
print('t-value: ',t,'\np-value: ',p,'\nconclusion: ',result)
```

    t-value:  1.5072186565119008 
    p-value:  0.06672104990276045 
    conclusion:  rethink strategy. There doesn't seem to be an alpha factor involved


The positive return generated by the portfolio based on this momentum strategy is not statistically significant at 95% confidence interval. But at 90% confidence level, the p-value is statistically significant and the null-hypothesis that the true mean is equal to zero is rejected. Based on this, I would be confident to pursue evaluation of this strategy further, though it might be a good idea to run the same steps on fairly comprehensive and updated data instead of a free sample, in order to get an accurate picture.
