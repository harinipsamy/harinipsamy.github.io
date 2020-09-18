---
title: Developing a Breakout strategy
tags:
   - quantitative finance
   - stock market
   - quantitative trading
---

Breakout strategy involves trading in stocks that have risen (or fallen) significantly above (or below) the average historical price range. 

## Variations in a breakout strategy 

### Bollinger Bands

Metrics of variability to define trading signals. Bollinger Bands - A fixed length rolling window to calculate mean and standard deviation of prices in that window and draw a line two standard deviations above and below the rolling mean to define upper and lower band.

### Pairs trading

Breakout strategy works well while combined with pairs trading strategy. Pairs trading strategy involves tracking two (or more) stocks that have similar characteristics and trading in the stocks if one deviates from it's usual behavior. For examply, Coco cola and Pepsi are two stocks that exhibit similar behavior and are expected to move in the same direction, provided other factors remain the same. Suppose Pepsi's stock price move up significantly and Coco cola's price movement remains unchanged, then it is fair to assume that the price of Pepsi will move down to the same proportion before this sudden movement. We can take advantage of this movement by shorting Pepsi and/or going long on Coco cola.

## Step 1: Define strategy / signal

Signal will be calculated based on the maximum high and minimum low of the stock prices in the rolling window. The size of the rolling window is defined by the lookback days.
Determine entry and exit signals and also the interval in which the stock positions will be evaluated to rebalance the portfolio.

## Step 2: Calculating breakout signal

a. calculate highs and lows of stock prices based on the lookback days
b. calculate long and short positions
    long: if Highs < closing price
    short: if lows > closing price

## Step 3: Calculate lookahead returns

## Step 4: Test for Significance

Use Kolmogorov-Smirnov test to determine significance of the performance of the strategy. If it is significant, then go ahead, implement the strategy. If not, look for a better strategy.
Test for significance with and without the outliers to reliably measure 
