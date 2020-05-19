---
title: "temp"
---

```python
import pandas as pd
import numpy as np
import glob
import os
from scipy import stats
```

# Momentum strategy 1: Selecting top and bottom performing stocks for long and short positions in the portfolio based on month-end observations.

### Read ticker data downloaded from quantquote


```python
files = glob.glob("/home/home/vbox_shared/projects/Momentum-Trading/daily/*.csv")
df = pd.concat([pd.read_csv(fp,names=['date','0','open','high','low','close','volume'])
                .assign(ticker=os.path.basename(fp).replace('_','.').split('.')[1]) for fp in files])
df['date'] = pd.to_datetime(df['date'],format='%Y%m%d')
print (df)

```

               date  0     open     high      low    close        volume ticker
    0    1998-01-02  0  23.5573  23.6455  23.2714  23.5573  7.108452e+06     ko
    1    1998-01-05  0  23.5784  23.6031  23.0279  23.4479  1.004965e+07     ko
    2    1998-01-06  0  23.2502  23.5149  23.2044  23.3808  7.462927e+06     ko
    3    1998-01-07  0  23.2255  23.3596  22.9855  23.3385  7.102218e+06     ko
    4    1998-01-08  0  23.2255  23.6455  23.2044  23.5361  8.561481e+06     ko
    ...         ... ..      ...      ...      ...      ...           ...    ...
    3921 2013-08-05  0  81.9700  82.4900  81.4600  82.3300  2.229676e+06    apa
    3922 2013-08-06  0  82.2900  82.4900  81.6200  82.2900  3.076233e+06    apa
    3923 2013-08-07  0  82.1200  82.5400  81.3500  81.9500  2.912846e+06    apa
    3924 2013-08-08  0  82.0000  83.5100  81.9200  83.1700  2.099414e+06    apa
    3925 2013-08-09  0  83.0400  83.5100  82.3800  83.2300  1.722495e+06    apa
    
    [1808278 rows x 8 columns]


### Pivot to closing prices


```python
close = df.pivot(index='date', columns='ticker', values='close')
```


```python
close
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
      <td>13.35110</td>
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
      <td>13.58530</td>
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
      <td>13.28170</td>
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
      <td>13.30420</td>
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
      <td>12.75330</td>
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
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2013-08-05</th>
      <td>45.87</td>
      <td>7.97962</td>
      <td>466.14200</td>
      <td>44.67</td>
      <td>59.01000</td>
      <td>36.4600</td>
      <td>90.9600</td>
      <td>74.18</td>
      <td>136.66</td>
      <td>47.65000</td>
      <td>...</td>
      <td>31.6700</td>
      <td>46.11000</td>
      <td>90.9911</td>
      <td>42.69000</td>
      <td>10.0700</td>
      <td>25.27</td>
      <td>27.67000</td>
      <td>74.78000</td>
      <td>29.9200</td>
      <td>83.94</td>
    </tr>
    <tr>
      <th>2013-08-06</th>
      <td>46.05</td>
      <td>7.88000</td>
      <td>462.30700</td>
      <td>44.83</td>
      <td>58.64000</td>
      <td>36.0600</td>
      <td>90.0400</td>
      <td>73.21</td>
      <td>135.59</td>
      <td>47.38000</td>
      <td>...</td>
      <td>31.3600</td>
      <td>45.56000</td>
      <td>90.8322</td>
      <td>42.54000</td>
      <td>9.9700</td>
      <td>25.42</td>
      <td>27.29000</td>
      <td>73.98000</td>
      <td>29.3700</td>
      <td>82.96</td>
    </tr>
    <tr>
      <th>2013-08-07</th>
      <td>46.30</td>
      <td>7.78000</td>
      <td>461.79100</td>
      <td>44.59</td>
      <td>58.77000</td>
      <td>35.8900</td>
      <td>89.8200</td>
      <td>73.03</td>
      <td>135.46</td>
      <td>47.10000</td>
      <td>...</td>
      <td>31.2200</td>
      <td>45.60000</td>
      <td>90.8223</td>
      <td>42.41000</td>
      <td>9.8800</td>
      <td>24.94</td>
      <td>27.41000</td>
      <td>73.90000</td>
      <td>29.0800</td>
      <td>82.61</td>
    </tr>
    <tr>
      <th>2013-08-08</th>
      <td>45.86</td>
      <td>7.91000</td>
      <td>461.06000</td>
      <td>45.05</td>
      <td>59.62000</td>
      <td>35.9300</td>
      <td>89.8800</td>
      <td>73.04</td>
      <td>134.47</td>
      <td>47.54000</td>
      <td>...</td>
      <td>31.0300</td>
      <td>45.57000</td>
      <td>91.2195</td>
      <td>42.77000</td>
      <td>9.9800</td>
      <td>24.98</td>
      <td>27.47000</td>
      <td>74.10000</td>
      <td>29.5400</td>
      <td>83.30</td>
    </tr>
    <tr>
      <th>2013-08-09</th>
      <td>46.24</td>
      <td>8.20000</td>
      <td>454.37000</td>
      <td>44.71</td>
      <td>59.50000</td>
      <td>35.8600</td>
      <td>89.7900</td>
      <td>73.53</td>
      <td>133.87</td>
      <td>47.15000</td>
      <td>...</td>
      <td>30.8400</td>
      <td>45.31000</td>
      <td>90.6400</td>
      <td>42.55000</td>
      <td>10.0200</td>
      <td>24.98</td>
      <td>27.65000</td>
      <td>74.41000</td>
      <td>29.4800</td>
      <td>82.56</td>
    </tr>
  </tbody>
</table>
<p>3926 rows × 500 columns</p>
</div>



### Check data for survivor bias


```python
a =close.isnull().sum()
a = a[a>0]
```


```python
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

### For each month-end observation period, rank the stocks by previous returns, from the highest to the lowest


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
port_ret
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
      <td>0.000000</td>
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
      <td>0.000000</td>
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
      <td>0.000000</td>
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
      <td>0.000000</td>
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
      <td>0.000000</td>
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
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2013-04-30</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2013-05-31</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.001256</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2013-06-30</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2013-07-31</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.00000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2013-08-31</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>188 rows × 500 columns</p>
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
