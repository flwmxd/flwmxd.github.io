---
title: Fluid Simulation
date: 2021-04-10 09:32:19
tags: Simulation
index_img : /images/fluid.jpg
---

This is an OpenGL Simulation Application, which written by c++.

They are assignments 4 from course Animation Simulation University of Leeds

## Repo is [here](https://github.com/flwmxd/Simulations)


# Project Descripton 

You will build a fluid simulator based on Smoothed 
Particle Hydrodynamics (SPH). It should come with a GUI where you can specify the initial state of the water and the environment. It should be able to enable/disable different forces. 

For your hard work, the Lady Partypooper will reward you for: 
1. A GUI to set up the simulation environment, run the simulation and render the results. (5g) 
2. A 2D SPH simulation with a water tank and a water blob floating in the air at first then freefalling under gravity (see Hint 1). The only forces considered here are gravity and internal 
pressure. The simulation should also consider the collisions between the particles and the water 
tank (but not between particles). The integration scheme should be the Leapfrog scheme. The 
kernel should be the poly6 kernel we introduced in the class. (8g) 
3. The same simulation as 2, but replacing the kernel with Debrun’s spiky kernel for pressure 
computation (only for pressure). (4g) 
4. The same simulation as 3, plus the viscosity and the viscosity kernel introduced in the class. (4g) 
5. The same simulation as 4, plus the surface tension with the poly6 kernel. (4g) 
The total reward is 5g + 8g + 4g + 4g + 4g = 25g. 

Hint 1: the initial state of the simulation: 
![fluid](/images/fluid/1.png)
Hint 2: the rendering can be performed via one of the following approaches: 
* drawing each particle as a small disc, or 
* using textures to fill each grid cell with transparencies controlled by the local particle density, or 
* marching cubes to draw the free surfaces 
Hint 3: the details are in the slides as well as **https://matthias-research.github.io/pages/publications/sca03.pdf**


# Particle Hydrodynamics

Smoothed-particle hydrodynamics (SPH) is a computational method used for simulating fluid flows. It was developed by Gingold and Monaghan (1977) and Lucy (1977) initially for astrophysical problems. It has been used in many fields of research, including astrophysics, ballistics, volcanology, and oceanography. It is a mesh-free Lagrangian method (where the coordinates move with the fluid), and the resolution of the method can easily be adjusted with respect to variables such as the density.




![ffd2d](/images/fluid.gif)
