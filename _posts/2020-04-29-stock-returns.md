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
* Log returns are great for compounding:
$100 grows to $100 (1 + .04/252)^252 = 104.08 at 4% interest compounded daily
which is close to $100 x e^0.04 = $104.08
log returns are also continuosly compounded returns.


* Numerical stability:
```ruby
np.prod(1/np.array(range(1,1000))) #gives 0.0
sum(np.log(1/np.array(range(1,1000)))) #gives -5905.2204..

```
* Additive:
Products turn to addition problem in log, which is easier for computing purposes. Also, product of two normally distributed variables is not normal, whereas the addition of two normally distributed variables also result in a normally distributed variable.

* Normally distributed:
log returns of a stock can reasonably be considered as normally distributed as long as they are identically and independantly distributed


