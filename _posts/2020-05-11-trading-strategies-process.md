---
title: "steps in developing trading stragies"
date: 2020-05-11
#layout: post
categories:
  - blog
tags:
  - building trading strategy
  - finance
 
---
# Steps
## Forming a hypothesis

Forming a hypothesis based on understanding of stock market and factors affecting the stocks in the stock universe.
example: retail stocks perform well during holiday season

## Research

Preliminary test of the hypothesis by determining what sort of long and/or short positions to enter at what time and list of rules for the strategy. We also test significance of the results by applying t-test to ensure returns are positive before proceeding to the next step.

## Testing

Consider investment size, cost of transactions, asset weights, acceptable risk, exit strategy, how frequently the trades are to be carried out

## backtesting

Backtesting the strategy on historical data to evaluate the performance.

## Sample types of trading strategies
1. Single Asset strategies
2. Pairwise strategies
3. Cross-sectional strategies, also known as equity statistical arbitrage/ equity market neutral strategy
4. Alternative data based strategies

# Forming a hypothesis
# Research
## Removing outliers
Extreme and unexpected values in market data which may or may not represent real events. Some common outliers are:
Fat finger errors - human error, missing data due to mistakes by the data provider. 
stock returns has to be calculated on total returns. usually ticker prices are adjusted for corporate actions like split, but not adjusted for dividends. We should also adjust the data for dividends, to enable comparison between two stocks on a comparable level.

