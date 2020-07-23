---
title: "Basic Statistics"

categories:
  - blog
tags:
  - building trading strategy
  - finance
  - datascience

---

This post covers some basic formulae in statistics.


Standard deviation: <img src="https://render.githubusercontent.com/render/math?math=\sqrt{\frac{\sum_{i=1}^{n} (x-\overline{\rm x})^2}{n}}">

Sample standard deviation: <img src="https://render.githubusercontent.com/render/math?math=\sqrt{\frac{\sum_{i=1}^{n} (x-\overline{\rm x})^2}{n-1}}">

Standard error: <img src="https://render.githubusercontent.com/render/math?math=\frac {sample%20\ standard%20\ deviation}{\sqrt{n}} ">

t-test: testing the null hypothesis that the population mean is 0.

<img src="https://render.githubusercontent.com/render/math?math=t%20=%20\frac%20{x-\mu}{sample\ %20%20SD_{\overline%20{x}}} = \frac%20{x}{sample\ %20%20SD_{\overline%20{x}}}">

<img src="https://render.githubusercontent.com/render/math?math=e^{i%20\pi}%20=%20-1">

Alpha research: observe a pattern, identify opportunity, verify using historical data using statistical tests

<img src="https://latex.codecogs.com/latex?R^2"> is a measure of goodness of fit. It tells us how much of the deviation from the predicted values can be explained by the variance. The higher the value, better. It is usually measured as a percentage, meaning, it ranges between 0 and 1. But there are cases where it is calculated slightly differently, and therefore can go negative. For example, sklearn's library calculates <img src="https://latex.codecogs.com/latex?R^2\ as\ \frac{1 - deviation from predicted values}{ variance}, or <img src="https://render.githubusercontent.com/render/math?math=R^2 = 1 - \frac{\sum_{i=1}^{n} (x-\tilde{x})^2 } {\sum_{i=1}^{n} (x-\overline{x})^2}"> , instead of just <img src="https://render.githubusercontent.com/render/math?math=R^2 = \frac{\sum_{i=1}^{n} (x-\tilde{x})^2 } {\sum_{i=1}^{n} (x-\overline{x})^2}">. R2 may also not be a good measure for accuracy if we are dealing with data that's not perfectly linear. When the difference between the actual values and predicted values is greated than the variance, it leads to a value > 1 and when you subtract it from 1, we get a negative value. 

