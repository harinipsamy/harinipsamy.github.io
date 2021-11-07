---
title: Confidence interval - for SC model

---

# Objective
An experiement was introduced to a geographical market, for example, opening a new showroom. let's call this treatment. 
We want to know what is the incremental effect of this treatment. we know markets that performs similarly to the treated market, 
we have sales data for treated market, as well as the similar markets from tn till t0. the treatment was introduced at time tm 
such that t0 > tm > tn.
Calculate sales for the treated entity based on similar markets for tm to t0. This is the sales that would have happened if 
there was no treatment. actual sales in the treated market/entity minus calculated sales is the incremental effect. 


  
# What I have

Inputs: y values for each time period in similar locations

|market |period   |  orders|
|-------|:--------|-------:|
|1      |1 (Jun 1)|      12|
|2      |1(Jun 1) |      14|
|3      | 1 (Jun 1) |17|
|1      | 2 (Jun 2) |9|
|2     |  2 (Jun 2) |6|
|3    |   2 (Jun 2) |12|
|1   |    3 (Jun 3) |9|
|2  |     3 (Jun 3) |6|
|3 |      3 (Jun 3) |12|

Given 3 locations that has similar characteristics, predict base line sales for 4th location from time tm (treatment start period) till time t0.
Caculate weights of each similar market to minimize distance from treated market's sales, using data from tn to tm. using this weight, calculate 
treated markets sales from tm to t0. 

# How?

calculate weights of each market (1-3) using sci.optimize.minimize. Apply this weight for calculating base-line sales from treatment period. 

# Next steps - calculating confidence interval

a lower and upper bound based on confidence interval for each prediction point. 

for example, if predicted baseline sales for tm = 15, and based on CI, the sale can be ~+-5, then we need to show 2 dotted lines the lower bound will be at 15-5 = 10 and 
upper bound will be at 15+5 = 20. This should be calculated for each time period till t0.

output is one set of weight. to calculate sales, we multiply weights by sales for each time period before and after treatment.



\
