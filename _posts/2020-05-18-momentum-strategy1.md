---
title: Momentum strategy based on historical performance

tags: 
 - quantitative research

---

In this post, we will go over implementing a simple momentum strategy. The strategy simply picks top and bottom performing stocks to buy/sell as the case maybe. 

## Read stock data

I downloaded free data from quantquote from here: https://quantquote.com/historical-stock-data. Quantquote provides free data for the 500 stocks in the current S&P 500 index going back to 1998 up till July 2013. It seems quite old but for the purpose of building a model and testing significance, this would suffice. If the model shows promise, we can test it further with current data.

<script src="https://gist.github.com/harinipsamy/a235de29ec8a85e9a0ede01bb8774a57.js"></script>







