---
title: Momentum strategy based on historical performance

tags: 
 - quantitative research
 - algorithmic trading
 - stock market
author: Harini Palanisamy
---

In this post, I will go over my implemention of a simple momentum strategy. The strategy simply picks top and bottom performing stocks to buy/sell as the case maybe. 

## Ticker data source

I downloaded free data from quantquote from [**here**](https://quantquote.com/historical-stock-data). Quantquote provides free data for the 500 stocks in the current S&P 500 index going back to 1998 up till July 2013. It seems quite old but for the purpose of building a model and testing its significance, this would suffice. If the model shows promise, I can test it further with current data.


## Importing libraries

First thing first. Let's import the libraries required. 


```python
import pandas as pd
import numpy as np
import glob
import os
from scipy import stats
```

### Reading ticker data downloaded from quantquote

Quantquote provides the stock price data for each symbol in a separate csv file. This requires and additional processing step and ensure we have all ticker data is available in a single table for analysis. The library glob will help in combining all ticker data and its glob.glob() method can be used on the type of file we want to collate - in this case, all the files with .csv extension. 

```python
files = glob.glob("/.../Momentum-Trading/daily/*.csv")
df = pd.concat([pd.read_csv(fp,names=['date','0','open','high','low','close','volume'])
                .assign(ticker=os.path.basename(fp).replace('_','.').split('.')[1]) for fp in files])
df['date'] = pd.to_datetime(df['date'],format='%Y%m%d')
print (df.head())

```

            date  0     open     high      low    close        volume ticker
    0 1998-01-02  0  23.5573  23.6455  23.2714  23.5573  7.108452e+06     ko
    1 1998-01-05  0  23.5784  23.6031  23.0279  23.4479  1.004965e+07     ko
    2 1998-01-06  0  23.2502  23.5149  23.2044  23.3808  7.462927e+06     ko
    3 1998-01-07  0  23.2255  23.3596  22.9855  23.3385  7.102218e+06     ko
    4 1998-01-08  0  23.2255  23.6455  23.2044  23.5361  8.561481e+06     ko

There's an extra column with 0s, but let's not worry about it at this time. We will only be working on closing prices anyway. 

### Pivot to closing prices

Having all the stock price information in a single dataframe makes it cumbersome to do operations on them. Since the strategy is based on time-series calculations on each ticker symbol separately before comparing them, having both date and ticker symbol on the same axis might not be a good idea. We can use groupby function of Pandas to perform the same functions, but pivot function renders a dataFrame as against groupby, which gives us a groupby object. Having the data as a dataframe will help take advantage of various time-series calculations made possible by pandas.

df.pivot() creates a reshaped dataframe by pivoting the original dataframe based on the arguments passed like index, column and values of the dataframe. We can create five dataframes each for open, high, low, close and volume using this method. But for now, we want only the closing prices, so we will create a dataframe of closing prices of all tickers. Since we are interested in time-series analysis, we will have date as the index and one column for each ticker.

```python
close = df.pivot(index='date', columns='ticker', values='close')
```

A glimpse of how the new dataframe looks like:

```python
close.iloc[:,:10].head()
```
```
ticker 	a 	aa 	aapl 	abbv 	abc 	abt 	ace 	acn 	act 	adbe
date 										
1998-01-02 	NaN 	13.3511 	3.95098 	NaN 	6.50799 	10.3555 	22.9865 	NaN 	32.06 	4.99041
1998-01-05 	NaN 	13.5853 	3.89020 	NaN 	6.40419 	10.4031 	22.8365 	NaN 	33.63 	5.05201
1998-01-06 	NaN 	13.2817 	4.60502 	NaN 	6.28477 	10.2311 	23.0180 	NaN 	33.44 	5.23685
1998-01-07 	NaN 	13.3042 	4.24032 	NaN 	6.34839 	10.2880 	23.1389 	NaN 	32.69 	5.15182
1998-01-08 	NaN 	12.7533 	4.39107 	NaN 	6.38299 	10.4507 	22.7446 	NaN 	33.38 	5.20604
```


### Check data for survivor bias

Before proceeding further, I will do a quick check on the data to ensure that it doesn't have survivor bias. 

Survivor bias is when we do caclulations on the stocks that are trading as of today. We consider only stocks that have 'survived' the past and ignore the stocks that are no longer in the stock environment that we are analyzing. For example, lot's of dot com companies that were listed in 1998 did not last after the bubble burst in the early 2000. If we were to do a 25 year analysis of past performance of stocks based on the stocks that are trading today, we would erroneously conclude that these stocks performed extremely well, basing our opinion on the few stocks like Amazon that survived the bubble. 

In order to get an accurate picture of market performance, we need to consider the stocks that were included in the stock environment at the beginning of our duration of analysis. Since our data is from 1998 to 2013, we need to make sure that the stock universe includes data that were in existence in 1998, but are no longer part of the stock universe, for whatever reason. 

I did the check by simply checking if there are stocks that had null values at the tail. This may not be absolutely foolproof, but it should suffice, considering the fact that we are still in the stage of evaluating whether our hypothesis has alpha and if we should proceed further to build the strategy. 

```python
# Count number of null values for each ticker
a =close.isnull().sum()
a = a[a>0]
```


```python
# Consider only those tickers that had null values in the last 20 dates. 

for i in a.index:
    if close[i].tail(20).isnull().sum() >0:
        print(i)

```

    cvh
    hnz
    pcs

The length of the tail I checked for null values is twenty - just a random number I used. There are three stocks in the universe which had no values at the end. It means they were removed from the stock universe for some reason, but we still have their past data. Great news!

```python
print(close['cvh'].tail(70))
```

    date
    2013-05-02    49.81
    2013-05-03    50.19
    2013-05-06    50.11
    2013-05-07      NaN
    2013-05-08      NaN
                  ...  
    2013-08-05      NaN
    2013-08-06      NaN
    2013-08-07      NaN
    2013-08-08      NaN
    2013-08-09      NaN
    Name: cvh, Length: 70, dtype: float64


Includes ticker with NaN values at tail. All is well.

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
