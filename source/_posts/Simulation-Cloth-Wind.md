---
title: Cloth Simulation
date: 2021-04-10 09:32:19
tags: Simulation
math: true
mermaid: true
index_img : /images/cloth.jpg


---

This is an OpenGL Simulation Application, which written by c++.

They are assignments 3 from course Animation Simulation University of Leeds

## Repo is [here](https://github.com/flwmxd/Simulations)


{% note-success %}
You will build a cloth simulation system to show different fabric behaviours. It should come with a GUI where you can load/write cloth mesh files (OBJ 
files). It should also be able to detect and resolve simple cloth-object collisions. 

For your hard work, the Mistress Fingercutter has graciously agreed to give you rewards for: 
1. A GUI to load OBJ files and render them in OpenGL (simple rendering without textures is 
acceptable). (2g) 

2. The same GUI to write OBJ files. This should write a single frame into an OBJ file.(2g) 

3. A mass-spring model to simulate a piece of cloth with arbitrary shapes specified by an OBJ file. 
The vertices are modelled as point masses and the edges are modelled as springs. The 
simulation is free falling onto the floor. (4g) 

4. Simulation Scenario 1 (SS1): a square piece of cloth floating horizontally in the air at first, which 
then free-falls onto a sphere on the ground. (4g) 

5. Simulation Scenario 2 (SS2): a square piece of cloth floating horizontally in the air first, then 
free-falling with two adjacent corners fixed in the space. (4g) 

6. SS1 with the sphere rotating in-place round the Y axis (up-axis) and friction between the sphere 
and the cloth. (3g) 

7. SS2 with wind blowing. (3g) 

8. Texture, lighting, and the ability to export the simulation result to a video. (3g) 


There are several places in town where you can find assistance. 
Hint 1: The Mesh Master at the Asylum of Geometry. (You can find the specs of OBJ files at 
https://en.wikipedia.org/wiki/Wavefront_.obj_file and some sample code in Minerva under Lab 
Resources) 

Hint 2: The Geomonger (a person who sells raw geometries) at the Mesh take-out place. (You can find other mesh software to help you debug and test your system. Meshlab: http://www.meshlab.net/) 

Hint 3: It is acceptable to export frames into images first, then use tools such as ffmpeg to make videos. 

Hint 4: The collision between the ball and the cloth should not be sticky. It can be implemented by a post-processing step to move any vertex inside the sphere to the closest point on the surface of the sphere. 

Hint 5: Wind blowing can be modelled as a steady force in a fixed direction. Hint 6: At each cloth vertex, the friction force between the sphere and the cloth is always in the opposite direction of their relative motions. The plane the friction force lies in is orthogonal to the surface normal of the sphere at that cloth vertex

{% endnote %}

# 1. Spring-Mass System

we consider the cloth as a collection of particles interconnected with three types of springs:

![springs](/images/mass-spring/1.jpg)

## 1.1 Particles

We assume the cloth contains n×n (evenly spaced) particles and use [***i***,***j***] (where 0≤*i*,*j*<n) to denote the particle located at the i-th row and j-th column. Each particle  [***i***,***j***] has the following states:

* **Mass** *m* (we assume all particles to have identical masses).
* **Position** $x_{i,j}(t)$.
* **Velocity** $v_{i,j}(t)$ which equals the derivate of $x_{i,j}(t)$ with respect to t: $v_{i,j}(t)$ = $\dot x_{i,j}(t)$.

## 1.2 Springs

There are three types of springs connecting all particles: structural, shear, and flexion (bend).

* **Structural**: each particle [***i***,***j***] is connected to (up to) four particles via structural connections: [***i***,***j+1***], [***i***,***j−1***], [***i+1***,***j***], [***i−1***,***j***].
* **Shear**: each particle [***i***,***j***] is connected to (up to) four particles via shear connections: [***i+1***,***j+1***], [***i+1***,***j−1***], [***i−1***,***j−1***], [***i−1***,***j+1***].
* **Flexion**: each particle [***i***,***j***] is connected to (up to) four particles via flexion connections: [***i***,***j+2***], [***i***,***j−2***], [***i+2***,***j***], [***i−2*** , ***j***].
Thus, each particle can have up to 12 directly connected neighbors.

**Stiffness**: in this project, we assume all structural springs to have stiffness K0, shear springs K1, and flexion strings K2.

**Rest length**: generally, a piece of cloth is considered to be in its rest state when there is no deformation (caused by stretching, shearing, or bending).

## 1.3 Forces

There are three types of forces you will need to consider for this project:

* **Spring forces**: given spring connecting two particles located at p and q with stiffness K and rest length L0, the spring force acting on p equals
$F_{spring}=K(L_0−∥p−q∥)\frac{p−q}{∥p−q∥}$.
This force changes over time as the particles move.

* **Gravity**: each particle is affected by (simple) gravity given by

$$F_G=\left( \begin{array} c 0 \ -mg \ 0 \end{array} \right) $$


* **Damping**: for a particle with velocity v, the amount of damping force it receives equals $F_{damp} =−c_d * v$ where $c_d$>0 is a constant. This force changes over time as $v$ changes.


## 1.4 Integration

Animating a mass-spring system involves solving an initial value problem. In particular, for each particle [i,j] you are given its mass m (which stays constant for all particles), initial position $x_{i,j}(0)$ and velocity $v_{i,j}(0)$. Given $Δt$>0, you need to use **Euler's method** to compute the position and velocity of each particle at time-steps $Δt, 2Δt, 3Δt$, ….

You need to pin two particles [ *n−1* ,*0*]  and [ *n−1*,*n−1*], which correspond to the upper left and right corners of the cloth. That is, the positions of these particles are fixed to their inital values. If no particle is pinned in place, the entire cloth will undergo a never-ending free fall because of gravity!

![cloth](/images/cloth-.gif)

## 1.5 Wind

Applying a wind force to the system involves applying a wind force a direction or directions in every particles.

$$ F =F_G + F_{wind} $$




### Cloth Wind
![wind](/images/cloth-wind.gif)



