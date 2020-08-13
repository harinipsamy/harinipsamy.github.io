---
title: 'Standardazing data'
tags:
  - Data Science
  - Analysis
  - Machine Learning
  - Factor modeling
  - Quantitative Finance
  
---

Standardizing data is a very import step in feature engineering. This is also crucial in factor modeling. In this post, we look at why and how it is done.



Some ways to standardize datasets

### De-meaning

Transform the data such that the mean of the transformed dataset is zero. This process is called de-meaning, as we subtract (or remove) the sample mean from each observation.

### Normalization

Scaling is a method used to normalize the range of values of features. It allows for comparison between two variables which otherwise may have different range of values, and to avoid giving too much importance to features with large values.  

There are different methods to scale features, based on the required purpose:

##### Min-max normalization

This is used to rescale the range of values. To rescale the data in range \[a,b], the following formula is applied for dataset x:

 <img src="https://latex.codecogs.com/svg.latex? a + \frac{(x-min(x))(b-a)}{max(x)-min(x)}"> 

##### Standardization

Feature standardization transforms the data to have mean = 0 and variance = 1. This is also called as z-score normalization

<img src="https://latex.codecogs.com/svg.latex? \frac{(x-\mu)}{\sigma},\ where\ \mu\ and\ \sigma\ are\ mean\ and\ standard\ deviation\ of\ the\ data,\ respectively."> 

##### Rescaling with absolute value

This transforms the data such the sum of absolute values is one. It's done by dividing each value by it's absolute value:

 <img src="https://latex.codecogs.com/svg.latex?\frac{x}{|x|}"> 



## Standardizing a factor in factor models

We can convert the raw factor values into a standardized factor. To standardize the factor, the following two conditions are required to be satisfied:

#### 1. Sum of weights should sum to 0

This is done by finding mean of factor values of all the stocks, and then subtracting mean from each of the factor values (de-mean).

<img src="https://latex.codecogs.com/svg.latex?x_i = x_i - \mu "> 

When the factor values are used as stock weights, we want the portfolio to be dollar neutral, that is, the dollar amount of all long positions is equal to the dollar amount of all short positions. This helps in testing the predictive power of the factor while excluding the influence of the overall market. A dollar neutral portfolio is close to market neutral. We assume that on an average beta of each stock is 1. 


#### 2. Sum of absolute value of weights should sum to 1 

This condition is achieved by rescaling values. Each value is divided by a scalar, which is sum of the absolute values. 

<img src="https://latex.codecogs.com/svg.latex?x_i = x_i/\lambda ">

Rescaling is done to make the leverage ratio equal to 1 for our portfolio. 

The above steps can be done in any order to standardize the factors.

