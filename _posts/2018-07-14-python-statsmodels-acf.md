---
layout: post
title: "Plot acf using statsmodels"
date: 2018-07-14 23:39:50
category: Statistics
tags: Statistics Time-series Python Statsmodels
author: dcf
mathjax: true
---
The sample autocorrelation function (acf) is simply ploted in R using the 
function acf(). To do the same task in Python, one convenient way is to
use plot_acf() in the package statsmodels.

{% highlight Python %}
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from statsmodels.tsa.stattools import acf
from statsmodels.graphics.tsaplots import plot_acf

np.random.seed(1)
# generate GARCH(1,1)
alpha0 = 0.1
alpha1 = 0.4
beta1 = 0.2
n = 10000 # T
w = np.random.randn(n)
epsilon = np.repeat(0.0, n)
h = np.repeat(0.0, n)
for i in range(1, n):
    h[i] = alpha0 + alpha1 * epsilon[i-1]**2 + beta1 * h[i-1]
        epsilon[i] = w[i] * np.sqrt(h[i])

acfunc, confint, qstat, pvalues = acf(epsilon, unbiased=False, nlags=40, qstat=True, fft=False, alpha=0.05, missing='none')
plot_acf(epsilon, lags=40)
{% endhighlight %}
