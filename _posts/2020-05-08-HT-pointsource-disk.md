---
title: "Temperature in a 2D-Disc with Point Heat Hource at the Center"
date: 2020-05-05
comments: true
categories:
  - Blog
tags:
  - Jupyter
  - Mathematics
  - Physics
---

The temperature change (ΔT) distribution, due a point source at the center, in a 2 dimensional disc has been derived analytically and can be represented as an exact function.
Note, that the boundary at the disc circumference is at a constant temperature (T<sub>0</sub>).
The [temperature change distribution is given using](https://www.comsol.com/model/download/646761/models.heat.localized_heat_source.pdf):

![function](\assets\images\HTfunc.JPG)

where α is the thermal diffusivity, given by dividing the thermal conductivity of a material with its specific heat capacity and the mass density. The instantaneous distance is `r` and the disc radius is `R`.

To plot the temperature distribution, I first create a 2D `meshgrid` with 551 points in x and y direction (-1 to 1) each using `meshx` and `meshy`.
Next, I get the distance of each point in the mesh from the source position and put it in a 2D matrix called `DIST`
Now, given the radius `R=1` for the disc, it is easy to get the temperature distribution by feeding in `DIST` inside the function above.

The `DIST`, and the corresponding tentative temperature distribution (still needs correction) are then plotted.

The max temperature change reaches a singularity at the source position. The max reported change is higher if a larger and larger mesh (with smaller step sizes in x and y) is used. To use not-a-very-large-mesh I just add a nanometer (`Qs`) to the existing `DIST` mesh with 551x551 values in it. This removes the 0 distance at the center and uses a finite value to start from. Hence the max temperature change shown is dependent on the `Qs`, or the size of the source. The smaller it is, the higher the max change in temperature. For infinitely small `Qs` the max temperature change tends to infinity - a singularilty.

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import axes3d
from matplotlib import cm
from matplotlib.colors import LightSource

meshx = 551
meshy = 551

kappa = 2200                # Thermal conductivity (W/(m.K))
rho = 4860                  # Density (Kg/m3)
C = 630                     # Specific heat capacity (J/(Kg.K))
alpha = kappa/(rho*C)       # Thermal diffusivity

x = (np.linspace(-1,1,meshx))                
y = (np.linspace(-1,1,meshy))

X,Y = np.meshgrid(x,y)
R=1

Qx = 0              # x position of the source
Qy = 0
Qcord = [Qx, Qy]
Qs = 1e-9
Q = 1
mult = Q/(2*np.pi*alpha)+Qs

def Tempr(distance,end):
    T = -np.log(distance/end)
    return T

distx = np.sqrt((X-Qx)**2)
disty = np.sqrt((Y-Qy)**2)

DIST = np.sqrt((distx**2+disty**2))

plt.figure(figsize = (7,7))
# The shape now only depends on the meshsizes.
plt.title('Plot of distance from source')
plt.imshow(DIST)
plt.colorbar()
plt.show()

plt.figure(figsize = (15,4))
plt.title('Distance of source')
plt.plot(Qd0, label='from up boundary')
plt.plot(Qd1, label='from right boundary')
plt.plot(Qd2, label='from down boundary')
plt.plot(Qd3, label='from left boundary')
plt.legend()
plt.show()

plt.figure(figsize = (7,7))
plt.title('$\Delta T$ plot')
plt.imshow(mult*Tempr(DIST,R), cmap='plasma')
plt.colorbar()
plt.show()
```
![Output 1](\assets\images\HTout1.png)
![Output 3](\assets\images\HTout3.png)

Then, to remove the negative values from the temperature grid calculated for all distances greater than R, and to plot the temperature surface in 3D, I use the following cell:

```python
Temps = mult*Tempr(DIST,R)
Temps[Temps<0]='nan'

plt.figure(figsize = (15,15))
ax = plt.axes(projection='3d')
ax.plot_surface(X,Y,Temps,linewidth=1, cmap='plasma', cstride=3, rstride=3, vmin=0, vmax=1200)
plt.title('$\Delta T$ vs. X and Y - Boundary at R={}'.format(R))
plt.show()
```
![Output 4](\assets\images\HTout4.png)
