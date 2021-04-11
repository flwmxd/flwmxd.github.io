---
title: Free-Form Deformation
date: 2021-04-10 09:32:19
tags: Simulation
index_img: /images/ffd.png
---

This is an OpenGL Simulation Application, which written by c++.

This is a assignments 1 from course Animation Simulation University of Leeds


## Repo is [here](https://github.com/flwmxd/Simulations)


{% note %}

* 1. A GUI to load, display, and write-out 2D triangular meshes. You can define your own file format. (6g) 

* 2. A 2D free-form deformation tool using a grid-based cage. The user should be able to use the mouse to move any vertex of the cage and deform a given 2D triangular mesh. The weights of a mesh vertex with respect to its associated cage vertices should be computed by bilinear interpolation. (6g) 

* 3. A 2D free-form deformation tool using a triangular-mesh cage. The user should be able to move the mouse to position any vertex of the cage and deform a given 2D triangular mesh. The weights of a mesh vertex with respect to its associated cage vertices should be computed by Barycentric coordinates. (4g) 

* 4. A systematic (grid-based or triangular) cage deformation. Different from 2 and 3, when the user moves one cage vertex, other cage vertices should move according to some arbitrarily defined functions, e.g. influence attenuation based on distance like what we taught in class. The given mesh will then be deformed accordingly. (4g) 

* 5. A 3D free-form deformation tool using a grid-based cage to deform a 3D mesh, similar to 2 in 2D, except that the weights of a mesh vertex with respect to its associated cage vertices should be computed by trilinear interpolation. (5g) 

{% endnote %}

# Features

### Barycentric coordinates

For $ P, t_1 + t_2 + t_3 = 1 $, and they are proportional to $△A_2A_3P, △A_1A_3P and △A_1A_2P $ All t > 0

![Barycentric](/images/ffd/Barycentric.png)


### Bilinear Interpolation
P and point out some of its properties:


$$
    P = \frac{(x_2 - x)(y_2 - y)}{(x_2 - x_1)(y_2 - y_1)} Q_{11} + \frac{(x - x_1)(y_2 - y)}{(x_2 - x_1)(y_2 - y_1)} Q_{21}
    + \frac{(x_2 - x)(y - y_1)}{(x_2 - x_1)(y_2 - y_1)} Q_{12} + \frac{(x - x_1)(y - y_1)}{(x_2 - x_1)(y_2 - y_1)} Q_{22}
$$

![bilinear](/images/ffd/bilinear.png)

In the ffd-2d, current selected point we called it as **Control Point**, if the cage is rectangle, using Bilinear Interpolation to calculate postion around it, otherwise it is triangle, using Barycentric coordinates to calculate


### FFD-2D

![ffd2d](/images/ffd2d.gif)

In the ffd-3d, using [Bernstein polynomial](https://en.wikipedia.org/wiki/Bernstein_polynomial) to implement


### FFD-3D
![FFD3D](/images/ffd3d.gif)
