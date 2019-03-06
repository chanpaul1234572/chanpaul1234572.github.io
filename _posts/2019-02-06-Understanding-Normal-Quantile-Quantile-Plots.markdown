---
title: "Understanding Normal Quantile-Quantile Plots(Q-Q Plots)"
layout: post
date: 2019-02-06
image: /assets/images/markdown.jpg
headerImage: false
tag:
- markdown
- elements
star: true
category: blog
author: Chan Paul
description: Understanding Normal Quantile-Quantile Plots(Q-Q Plots)
---

<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

# Understanding Normal Quantile-Quantile Plots(Q-Q Plots)

## What is a Quantile-Quantile Plots
`If a set of observations is approximately normally distributed, a normal quantile-quantile(QQ) plot of the observations will result in an approximately straight line.`

- Why we need Q-Q plot: 
    - **To assess data normality**
    - As many statistical inference procedures assume we are sampling from a normally distributed population
    - Check the data are reasonable or not.

## How to construct a Q-Q plot

- Given a set of data: `3.89, 4.75, 6.33, 4.75, 7.21, 5.78, 5.80, 5.20, 6.64`
- First, we sort the data from smallest to largest:
`3.89, 4.75, 4.75, 5.2, 5.78, 5.8, 6.33, 6.64, 7.21`
- Second, we plot these data against appropriate quantiles from the standard normal distribution.
    - For this example, we got n = 9 data, so we are going to place n = 9 values down here for a standard normal random variable, such that the distribution is split up into **n+1 = 10 equal area**.

```python
import matplotlib.pyplot as plt
import numpy as np
import matplotlib.mlab as mlab
import math
from scipy.stats import norm
%matplotlib inline

def cal(x):
    return (math.exp(-(x * x)/2)) / (math.sqrt(2 * np.pi))

mu = 0
variance = 1
sigma = math.sqrt(variance)
x = np.linspace(mu - 3*sigma, mu + 3*sigma, 100)
plt.plot(x,mlab.normpdf(x, mu, sigma))
for i in range(0, 9, 1):
    #ppf is the inverse function of cdf, so given a cdf, we can find where is the x
    x = norm(0, 1).ppf(0.1 + i * 0.1)
    y = cal(x)
    print(x, y)
    plt.plot([x, x], [0, y], 'k-')
plt.savefig("norm.png")
plt.show()
```
 

```bash
The x and y value :

-1.2815515655446004 0.17549833193248685
-0.8416212335729142 0.2799619204078084
-0.5244005127080407 0.3476926142000738
-0.2533471031357997 0.38634253349686054
0.0                 0.3989422804014327
0.2533471031357997 0.38634253349686054
0.524400512708041  0.34769261420007375
0.8416212335729143 0.2799619204078083
1.2815515655446004 0.17549833193248685
```

![](https://i.imgur.com/HKYJkde.png)

- If we sampling 9 values from the standard normal distribution, we would expect the smallest value is around -1.28 and the second smallest is around -0.84 and so on and so on until the largest which is around 1.28
- So, the third step, we plot the smallest value of the data, which is 3.89 against with -1.28, the second smallest value against with -0.84 and so on ans so on
- If our data is approximately normally distributed, then the result should be an approximately straight line.
- We can use other normal distribution instead of standard normal distribution, the scaling is different but the result should be the same.

```python=3.6
l = [3.89, 4.75, 4.75, 5.2, 5.78, 5.8, 6.33, 6.64, 7.21]
k = 0
for y in l:
    x = norm(0, 1).ppf(0.1 + k * 0.1)
    plt.plot(x, y, 'bo')
    k += 1
plt.show()
```

![](https://i.imgur.com/MlI0Pyl.png)

- Loosely speaking, the x-axis is the value we expect to get from the standard normal distribution.
- We plotted the $i$th ordered value against the $\frac{i}{n+1}$th quantile of the standard normal distribution.
- For example. 3.89 is the $\frac{1}{9+1} = 0.1$ quantile of the standard normal curve

## Other Variety of Q-Q plots
- Instead of using $\frac{i}{n+1}$, we can use $\frac{i-\frac{1}{2}}{n}$, or more generally $\frac{i - a}{n + 1 - 2a}$, where $a\in[0, \frac{1}{2}]$

## Analysis 
- For a normal distribution result:

```python=3.6
#Generated from random.org 
l_normal = [1.3086826620e+0,
 7.8752650560e-1,
-3.9277626740e-1,
-6.3501145360e-1,
 1.3714005600e+0,
 4.6274928280e-1,
 1.9313073180e+0,
 2.5144777760e+0,
 6.1565012250e+0,
-5.7757600360e-1,
-3.2145715660e-1,
-5.5478012630e-1,
-1.6087961560e+0,
 2.1849299450e+0,
 2.6801977930e+0,
 2.9745528740e+0,
-5.6551089930e-1,
-7.2461047730e-1,
-1.6510428600e-3,
 1.0807244730e+0,
 2.9665445540e+0,
 3.4942761060e+0,
 3.4678777510e+0,
 7.2744971950e-1,
-1.6344429980e+0,
-9.8987861530e-1,
 1.1936872200e-1,
 5.1202092140e+0,
 2.1107856220e+0,
 2.3443140720e+0,
-6.3024726800e+0,
-1.3706874030e+0,
-3.6406160640e+0,
 1.8804573320e+0,
-3.1201811780e+0,
 4.7946945660e-1,
-4.9668393460e-1,
-3.2398291930e+0,
 3.3415231020e-1,
-1.9575301340e+0,
-1.6437609390e+0,
 2.2975881940e+0,
 4.5455132930e+0,
-1.4566417400e+0,
-3.7541139280e+0,
-3.6263327350e+0,
-1.4311703160e+0,
-2.6039969640e+0,
-4.0731627820e+0,
 1.0877776900e+0,]

k = 0
#How many quantile we need to seperate
r = 1 / len(l_normal)
for y in sorted(l_normal):
    x = norm(0, 1).ppf(r + k * r)
    plt.plot(x, y, 'bo')
    k += 1
line = [[norm(0,1).ppf(r * 3), norm(0,1).ppf(r*46)], [sorted(l_normal)[2], sorted(l_normal)[45]]]
slope = (line[1][1] - line[1][0]) / (line[0][1] - line[0][0])
intercept = line[1][0] - slope * line[0][0]
axes = plt.gca()
x_vals = np.array(axes.get_xlim())
y_vals = intercept + slope * x_vals
plt.plot(x_vals, y_vals, '-r')
plt.xlabel("Theoretical Quantiles")
plt.ylabel("Sample Quantiles")
plt.savefig("Normally_distributed_data.png")
plt.show()

```
![](https://i.imgur.com/CNnzlGB.png)


- Pretty stright line

- For a random distribution result:

```python=3.6
l_random = [-92, -91, -84, -83, -80, -77, -76, -72, -70, -65, -64, -59, -53, -52, -51, -46, -42, -41, -38, -33, -30, -28, -26, -25, -21, -19, -17, -4, -1, 5, 8, 10, 22, 23, 24, 25, 29, 34, 46, 57, 59, 60, 64, 66, 69, 71, 83, 87, 92, 97]
k = 0
r = 1 / len(l_random)
for y in sorted(l_random):
    x = norm(0, 1).ppf(r + k * r)
    plt.plot(x, y, 'bo')
    k += 1
plt.xlabel("Theoretical Quantiles")
plt.ylabel("Sample Quantiles")
plt.savefig("Randomly_distributed_data.png")
plt.show()
```

![](https://i.imgur.com/EXCEXxU.png)
- Not stright line


-For a uniformly distributed result:
![](https://i.imgur.com/ptyhUtk.png)

- For a Right-skewed distribution result:
- ![](https://i.imgur.com/1Dh3tDd.png)

- Heavier tails than normal distribution 
![](https://i.imgur.com/No4dIo4.png)

## Remark

- Sometimes, there are some natural variability that will lead to us seeing some curvature or major outliner.
- For a smaller sample size, the result will be more extreme.


