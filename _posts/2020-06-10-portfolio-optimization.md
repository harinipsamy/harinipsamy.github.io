---
title: "Portfolio Optimization"
tags:
  - quantitative finance
  - portfolio management
  - stocks
  - algorithmic trading

---


Optimization involves finding maximum or minimum of a funtion. A Maximization problem can be converted to a minimization problem by optimizing for the negative of a function.

## Finding Minimum 

At the point where the function is minimum, the slope is 0. At all other points, the slope is either some positive or negative number.

By setting the derivative of the function to 0 and solving for x, we can find the minimum point.


### Local minimum

To distinguish between minima/maxima or saddle points, we can use the second derivative test:
<img src="https://latex.codecogs.com/svg.latex?if\ f''(x_0) > 0, then\ f\ has\ a\ local\ minimum\ at\ x_0">

For a function of two variables, we must use hessian matrix instead of second order derivative.

## Convex functions

A function is convex when any line segment drawn between two points on the function lie above the graph. A convex function has only one minimum. When the objective function and the inequality constraints are convex and the equality constraints have the form <img src="https://latex.codecogs.com/svg.latex?\ f(x) = a^Tx+b">, the optimization problem is called as convex optimization problem.

## Two-Asset portfolio optimization

### Objective funtion

We generally want high returns with low variance. So To optimize a portfolio, the objective function will be:
<img src="https://latex.codecogs.com/svg.latex? \textbf{objective:}\ minimize: -x^T\mu + bx^TPx,\ b = trade-off\ parameter ">

b is subjective and represents the percent return the individual is willing to forego for each unit of variance.

We can also have objective funtion that only maximizes returns or minimizes variance and add the variance or returns equation as a constraint. 

### Constraints

We can have any number of constraints based on the portfolio strategy. Some common ones are:

**Weights sum to one:**
<img src="https://latex.codecogs.com/svg.latex? \sum{x_i} = 1">

**No short selling:**
<img src="https://latex.codecogs.com/svg.latex? 0\leq x_i \geq 1">

**sector limits:**
<img src="https://latex.codecogs.com/svg.latex? x_biotech \leq M,\ \ M = max. % of portfolio to invest in biotech companies">

## Rebalancing portfolio

We need to periodically rebalance the portfolio to reflect the desired weights for each stock calculated using portfolio optimization. To rebalance, we need to first run the optimization algorithm using the same objective function and constraints but using updated data. 

### Rebalancing frequency
The frequency at which the rebalancing should be done depends on the costs we are willing to incur. Frequently updating the weights of the stocks will result in higher rebalancing costs, as transactions costs and taxes are incurred each time a trade takes place.

### Rebalancing event

**Temporal:** Portfolio is rebalanced at a pre-determined time - daily, bi-weekly, monthly, etc

**Threshold:** Portfolio is rebalanced when a pre-determined threshold of a parameter is breached. For example, when weight of the assets in the portfolio changes by more than 10%

**combined:** Rebalancing event is when both temporal and threshold events occur.
