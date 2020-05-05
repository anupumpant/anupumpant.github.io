---
title: "Plotting an Interesting Function in Jupyter Notebook"
date: 2020-05-05
comments: true
categories:
  - Blog
tags:
  - Jupyter
  - Mathematics
---

The convergence of the summation of the following series is still an open problem in Mathematics.

So I thought it would be an interesting exercise to visualize it and form an intuition.

### Plotting the funtion:

$y=\frac{1}{x^3\sin^2(x)}$

```python
import numpy as np
import matplotlib.pyplot as plt
from pylab import rcParams

# For setting the size of the figure
# [horizontalsize, vertical size]
rcParams['figure.figsize'] = [15,5]

# Creates an array x with 1000000 numbers from 1 to 10^3,
# equally spaced in log-space
x = np.logspace(1,3,1000000)
# Calculates the value of y for each x.
y = (1/(x**3*np.sin(x)**2))

# Plots a lineplot of y vs. x
plt.plot(x,y)
# Changes the y and x-axes to logarithmic axes
plt.yscale('log')
plt.xscale('log')
# Sets the title of the plot
plt.title("y=1/(x**3*np.sin(x)**2)")
# Shows the plot
plt.show()
```

![png](/assets/images/output_1_0.png)

```python
# Creates an array x with 100000 numbers from 1 to 300
x = np.linspace(1,300,100000)
# Calculates the value of y for each x.
y = (1/(x**3*np.sin(x)**2))

# Plots a lineplot of y vs. x
plt.plot(x,y)
# Changes the y-axis to logarithmic axis
plt.yscale('log')
# Sets the title of the plot
plt.title("y=1/(x**3*np.sin(x)**2)")
# Shows the plot
plt.show()
```

![png](/assets/images/output_2_0.png)
