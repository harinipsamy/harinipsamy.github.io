I. Theory
1. stock data
2. liquidity
3. volatility
4. Markets in different timezones
5. Arbitrage
6. survivor bias
7. raw and log returns

II. calculations/adjustments
1. OHLC
2. resampling
3. Filling gaps in market data
4. Distribution of prices and returns


Stock returns are calculated as r = (Pt - Pt-1)/Pt-1
log return or natural log return R = ln(Pt/Pt-1)
If the value of r is small (|r| <<1 , ln(r+1) ~ r

why log returns?
* Numerical stability:
```ruby
np.prod(1/np.array(range(1,1000))) #gives 0.0
sum(np.log(1/np.array(range(1,1000)))) #gives -5905.2204..

```
