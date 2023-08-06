---
title: Fake-Nanite
date: 2023-08-01 23:00:00
tags: Geometric Processing
index_img : /images/nanite/virtual_geometry.png
sticky: 100
---

# Introduction

Implement Nanite in my personal Game Engine( it is an experimental function now).  
the goal is to render more than 10 billion faces.

## PPT [Click Here](../../../../nodeppt/dist/nanite.html)

<p align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/79aaFzgOso0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</p>

# GPUDriven Pipeline

Due to the increasingly high performance requirements of AAA games, the traditional rendering pipeline process cannot satisfy the real-time performance of the current AAA games. Therefore, in recent years, almost all engines will perform rendering-related operations on the GPU. one of the main purposes is to reduce the latencies between CPU-GPU, and at the same time release the CPU, so that the CPU can focus more on other tasks (AI, physical computing, network...)

## Triditional Rendering

## Bottleneck of Triditional Rendering

- High CPU overload
   - Frustum/Occlusion Culling
   - Prepare drawCall
- GPU idle time
    - CPU can not follow up GPU
- High driver overhead
    - GPU state exchange overhead when solving large amount of drawcalls

## GPUDriven Rendering

- GPU decides what objects are actually drawed
   - LOD Selection
   - Visiblity Culling on GPU
- No CPU/GPU roundtrip

## What Assassins Creed did ?

- Use **mesh cluster** rendering

- In some cases, the culling system is not efficiency. 
   - it could case overdraw, but parts of triangles players can not see it.

# Cluster 

- It is the minimum culling unit. 
   - each **Cluster** contains 128 triangles.

# Cluster Group

- it is the simplification unit
   - each **Cluster Group** contains 128 triangles.

# QEM algorithm

The central idea of the algorithm is to iteratively remove edges in the mesh through a process known as edge contraction which merges the vertices at an edge's endpoints into a new vertex that optimally preserves the original shape of the mesh. This vertex position can be solved for analytically by minimizing the squared distance of each adjacent triangle's plane to its new position after edge contraction. With this error metric, edges can be efficiently processed using a priority queue to remove edges with the lowest cost until the mesh has been sufficiently simplified.

# Culling

## Frustum Culling

## Occlusion Culling 

# Visibility Buffer

## Overdraw

## Materil ID
