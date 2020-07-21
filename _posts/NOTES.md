notes for future posts - created from course material 


alpha factors : drivers of mean returns
risk factors : drivers of volatility

factors can be based on: momentum, fundamental info, signals frm social media. - process them in a way that is predictive of future movement of stocks

## Standardizing a factor

satisfy 2 conditions:
1. sum of wts sum to 0 -> find mean of factor values. subtract mean from each factor values (de-mean)
2. sum of abs val of wts sum to 1 -> rescale val: divide each value by a scalar

can be done in any order

### why?
1. de-meaning
factor values will be used as stock weights. we want the portfolio to be dollar neutral ($ long = $ short). A $ neutral port., is approx market neutral. we assume that On avg., beta of each stock = 1. (beta = how much stock moves when the market moves)

Notional/trade book value = $ amt associated with port. : $ val. of inv. in a stock = wt. of stock in the port * notional

In theoritical $ neutral port, inv = $0. but in real life, there are margin cost for shorting positions, transac cost, etc

converting port to $ neutral port.:

port: A = 0.6 B = 0.4 -> this is heavier on the positive side
$ neutral port: shift the stock wts until the wts in positive side and negative side are balanced. -> A=0.1 B = -0.1

the relative difference of the wts of stocks in both port will be same: 0.6-0.4 = 0.2 and 0.1-(-0.1) = 0.2

This is achieved by de-meaning ( taking average of the wts and subtracting the avg from each of the wts)

2. rescaling
to make the leverage ratio =1 for our port.

leverage: borrowing to invest -> either by borrowing cash or taking short positions













## Rough - copied frm discarded materials for posts

### Forecasting of AR,MA, ARMA models

## vector autoregressive model.



# ML methods - Kalman & particle filters

particle filters do not assume normality and can work with non-linear data

# ML methods - RNN

used for NLP and time series. output of one regression is fed into input of another regression



<!-- wp:heading -->
<h2>NLP Pipeline</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>Text processing -&gt; feature extraction -&gt; modeling</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>Text processing</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>html tags, urls, punctuations, highercases, stop words</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4>Capturing text data</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3><strong>Feature extraction</strong></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>computers don't understand words. we need to feed the features (or importance) of words to build models</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sorted(counts.items(),key=lambda count:count&#91;1])</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:group -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:group -->

<!-- wp:heading {"level":4} -->
<h4>Ranking based on word count
</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We can list the most and least common words in a text by using a simple for loop.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>#sort words based on counts:</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>sorted_word_counts = </code><br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->



