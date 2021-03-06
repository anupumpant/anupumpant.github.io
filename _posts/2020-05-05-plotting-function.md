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

The convergence of the summation of the following series is still an open problem in Mathematics. See the [Twitter thread](https://twitter.com/fermatslibrary/status/1245343415010811906) for more discussion on this.

So I thought it would be an interesting exercise to visualize it and form an intuition.

### Plotting the funtion:

<img src="/assets/images/func_1.JPG" height="50" />

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

### Now, plotting the partial sums

![png](/assets/images/func_2.JPG)

```python
maxn = 100000
x = np.linspace(1,maxn,maxn)
y = (1/(x**3*np.sin(x)**2))

ysum = np.zeros(len(x))
for i in range(0,len(x)):
    ysum[i] = np.sum(y[:i])

plt.plot(x,ysum)
print(ysum[-1])
```

30.314520869819052

```python
%matplotlib notebook
plt.plot(x,ysum)
plt.yscale('log')
```
![png](/assets/images/output_3_0.png)

So far I have been able to compute it till n = 50 billion.

**For n = 50,000,000,000, the value is 30.31454615893876.**

![JPG](/assets/images/partialsums_func2.JPG)

I did it with piecewise calculations, as a single large calculation kept crashing the program because it ran out of memory.
I used the following cell.

```python
def ysum(minn,maxn):
    x = np.linspace(minn,maxn,maxn-minn+1)
    #print(x[10])
    y = (1/(x**3*np.sin(x)**2))
    ysum = np.sum(y)
    return ysum

start = 1
maxn = 1E11
x1 = np.arange(start=start,stop=maxn, step=1E8)
x2 = x1+1E8-1

sum=0
for i in range(0,len(x1)):
    sum = sum+ysum(x1[i],x2[i])
    print(x1[i],'\t', x2[i],'\t',sum)
```

Although, all of that might be for nothing (except for having learned some Jupyter), as it seems like some part of it may have been resolved. The paper below is beyond what I can possibly ever understand.

[On convergence of the Flint Hills series.](https://arxiv.org/pdf/1104.5100.pdf)

Found on [this thread](https://mathoverflow.net/questions/24579/convergence-of-sumn3-sin2n-1) at math overflow.
