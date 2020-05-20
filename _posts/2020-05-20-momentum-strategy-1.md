---
title: Momentum strategy based on historical performance

tags: 
 - quantitative research
author: Harini Palanisamy
---

In this post, we will go over implementing a simple momentum strategy. The strategy simply picks top and bottom performing stocks to buy/sell as the case maybe. 

## Ticker data source

I downloaded free data from quantquote from [**here**](https://quantquote.com/historical-stock-data). Quantquote provides free data for the 500 stocks in the current S&P 500 index going back to 1998 up till July 2013. It seems quite old but for the purpose of building a model and testing significance, this would suffice. If the model shows promise, we can test it further with current data.


## Importing libraries

First thing first. Let's import the libraries we would require. 


```python
import pandas as pd
import numpy as np
import glob
import os
from scipy import stats
```

### Reading ticker data downloaded from quantquote

Quantquote provides the stock price data for each symbol in a separate csv file. In order to put all the ticker data together, we can use a library called glob, and use the method called glob.glob() on the type of file we want to collate - in this case, all the files with .csv extension. 

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

Having all the stock price information in a single dataframe makes it cumbersome to do operations on them. Since we will be performing timer-series calculations on each ticker symbol, having both date and ticker symbol on the same axis might not be a good idea. We can also use groupby function of Pandas to perform the same functions, but pivot function renders a dataFrame as against groupby, which gives us a groupby object, which might not be the best option when we want to do repeated calculations.

df.pivot() creates a reshaped dataframe by pivoting the original dataframe based on the arguments passed like index, column and values of the dataframe. We can create five dataframes each for open, high, low, close and volume using this method. But for now, we want only the closing prices, so we will create a dataframe of closing prices of all tickers. Since we are interested in time-series analysis, we will have date as the index and one column for each ticker.

```python
close = df.pivot(index='date', columns='ticker', values='close')
```

A glimpse of how the new dataframe looks like:

```python
close.head()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>ticker</th>
      <th>a</th>
      <th>aa</th>
      <th>aapl</th>
      <th>abbv</th>
      <th>abc</th>
      <th>abt</th>
      <th>ace</th>
      <th>acn</th>
      <th>act</th>
      <th>adbe</th>
      <th>...</th>
      <th>xl</th>
      <th>xlnx</th>
      <th>xom</th>
      <th>xray</th>
      <th>xrx</th>
      <th>xyl</th>
      <th>yhoo</th>
      <th>yum</th>
      <th>zion</th>
      <th>zmh</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1998-01-02</th>
      <td>NaN</td>
      <td>13.3511</td>
      <td>3.95098</td>
      <td>NaN</td>
      <td>6.50799</td>
      <td>10.3555</td>
      <td>22.9865</td>
      <td>NaN</td>
      <td>32.06</td>
      <td>4.99041</td>
      <td>...</td>
      <td>40.6169</td>
      <td>7.96400</td>
      <td>21.6592</td>
      <td>9.04339</td>
      <td>30.5852</td>
      <td>NaN</td>
      <td>4.14437</td>
      <td>6.14229</td>
      <td>36.2098</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-01-05</th>
      <td>NaN</td>
      <td>13.5853</td>
      <td>3.89020</td>
      <td>NaN</td>
      <td>6.40419</td>
      <td>10.4031</td>
      <td>22.8365</td>
      <td>NaN</td>
      <td>33.63</td>
      <td>5.05201</td>
      <td>...</td>
      <td>40.8146</td>
      <td>7.80886</td>
      <td>21.4182</td>
      <td>8.77893</td>
      <td>31.0476</td>
      <td>NaN</td>
      <td>3.92563</td>
      <td>5.99731</td>
      <td>36.9102</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-01-06</th>
      <td>NaN</td>
      <td>13.2817</td>
      <td>4.60502</td>
      <td>NaN</td>
      <td>6.28477</td>
      <td>10.2311</td>
      <td>23.0180</td>
      <td>NaN</td>
      <td>33.44</td>
      <td>5.23685</td>
      <td>...</td>
      <td>40.6923</td>
      <td>7.75715</td>
      <td>20.6111</td>
      <td>8.75765</td>
      <td>30.7898</td>
      <td>NaN</td>
      <td>3.99250</td>
      <td>5.75640</td>
      <td>36.5122</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-01-07</th>
      <td>NaN</td>
      <td>13.3042</td>
      <td>4.24032</td>
      <td>NaN</td>
      <td>6.34839</td>
      <td>10.2880</td>
      <td>23.1389</td>
      <td>NaN</td>
      <td>32.69</td>
      <td>5.15182</td>
      <td>...</td>
      <td>39.9766</td>
      <td>7.42204</td>
      <td>21.2435</td>
      <td>8.70293</td>
      <td>29.7423</td>
      <td>NaN</td>
      <td>3.98813</td>
      <td>5.75640</td>
      <td>35.4618</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-01-08</th>
      <td>NaN</td>
      <td>12.7533</td>
      <td>4.39107</td>
      <td>NaN</td>
      <td>6.38299</td>
      <td>10.4507</td>
      <td>22.7446</td>
      <td>NaN</td>
      <td>33.38</td>
      <td>5.20604</td>
      <td>...</td>
      <td>39.4021</td>
      <td>7.45928</td>
      <td>20.8068</td>
      <td>8.64518</td>
      <td>29.1286</td>
      <td>NaN</td>
      <td>4.01562</td>
      <td>5.53041</td>
      <td>32.6286</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 500 columns</p>
</div>



### Check data for survivor bias

Before we proceed further, let's do a check on the data we downloaded to ensure that it doesn't have survivor bias. Survivor bias is when we do caclulations on the stocks that are trading as of today. In other words, we consider only stocks that have 'survived' the past and ignore the stocks that are no longer in the stock environment that we are analyzing. For example, lot's of dot com companies that were listed in 1998 did not last after the bubble burst in the early 2000. If we were to do a 25 year analysis of past performance of stocks based on the stocks that are trading today, we would erroneously conclude that these stocks performed extremely well, basing our opinion on the few stocks like Amazon that survived the bubble. In order to get an accurate picture of market performance, we need to consider the stocks that were included in the stock environment at the time we start of our duration of analysis. Since our data is from 1998 to 2013, we need to make sure that the stock universe includes data that were in existence in 1998, but are no longer part of the stock universe, for whatever reason. 

I did the check by simply checking if there are stocks that had null values at the tail. This may not be absolutely foolproof, but it should suffice, considering the fact that we are still in the stage of evaluating whether our hypothesis has alpha and if we should proceed further to build the strategy. 

```python
# Count number of null values for each ticker
a =close.isnull().sum()
a = a[a>0]
```


```python
# consider only those tickers that had null values in the last 20 dates. Twenty is just a random number I used. There are three stocks in the universe which had no values at the end, which is what we were looking for.

for i in a.index:
    if close[i].tail(20).isnull().sum() >0:
        print(i)

```

    cvh
    hnz
    pcs



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



For each month-end observation period, we rank the stocks by previous returns to find the best and worst performing stocks during the period. Pandas has resample method that converts the data into a different time factor. 


```python
monthly = close.resample('M').last()
returns = np.log(monthly) - np.log(monthly.shift(1))
prev_returns = returns.shift(1)
lookahead_returns = returns.shift(-1)
```


```python
# rank the stocks by prev returns from highest to lowest
def rank_(prev_returns, n):
    top_prices = pd.DataFrame(0, index=prev_returns.index, columns=prev_returns.columns)
    for index, col in prev_returns.iterrows():
        top_prices.loc[index, col.nlargest(n).index] = 1
    return top_prices

```

### Select the top performing stocks for the long position, and the bottom performing stocks for the short position


```python
# select top for long, bottom for short
long = rank_(prev_returns, 10)
short = rank_(-1 * prev_returns, 10) 

```


```python
# calculate expected portfolio returns
port_ret = (long * lookahead_returns - short * lookahead_returns)/(10+10)
port_ret.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>ticker</th>
      <th>a</th>
      <th>aa</th>
      <th>aapl</th>
      <th>abbv</th>
      <th>abc</th>
      <th>abt</th>
      <th>ace</th>
      <th>acn</th>
      <th>act</th>
      <th>adbe</th>
      <th>...</th>
      <th>xl</th>
      <th>xlnx</th>
      <th>xom</th>
      <th>xray</th>
      <th>xrx</th>
      <th>xyl</th>
      <th>yhoo</th>
      <th>yum</th>
      <th>zion</th>
      <th>zmh</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1998-01-31</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-02-28</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-03-31</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>-0.000456</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-04-30</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00928</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>-0.004008</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1998-05-31</th>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.018121</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 500 columns</p>
</div>



## statistical tests


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
port_ret.T.sum().mean()
```




    0.0060848424554046255




```python
(np.exp(port_ret.T.sum().mean() * 12 ) -1 ) * 100
```




    7.575001799545111




```python
t, p , result= t_test(port_ret.T.sum())
```


```python
print('t-value: ',t,'\np-value: ',p,'\nconclusion: ',result)
```

    t-value:  1.5072186565119008 
    p-value:  0.06672104990276045 
    conclusion:  rethink strategy. There doesn't seem to be an alpha factor involved



```python

```
