title: Fake Nanite Implementation
speaker: Tian
plugins:
    - echarts

<slide class="bg-black-blue aligncenter" image="/images/nanite/nanite.png .drak">

# Fake Nanite Implementation {.text-shadow}
 
# {.text-intro}

[:fa-youtube: Youtube](https://www.youtube.com/watch?v=79aaFzgOso0){.button.ghost}
[:fa-github: Github](https://github.com/flwmxd){.button.ghost}

<slide class="bg-black-blue slide-top">

:::{.content-left} 
### GPU-Driven Pipeline

Due to the increasingly high performance requirements of AAA games, the traditional rendering pipeline process cannot satisfy the real-time performance of the current AAA games. Therefore, in recent years, almost all engines will perform rendering-related operations on the GPU. one of the main purposes is to reduce the latencies between CPU-GPU, and at the same time release the CPU, so that the CPU can focus more on other tasks (AI, physical computing, network...)

<slide class="bg-black-blue slide-top">
:::{.content-left} 

## Triditional Rendering

## Bottleneck of Triditional Rendering

- High CPU overload
   - Frustum/Occlusion Culling
   - Prepare drawCall
- GPU idle time
    - CPU can not follow up GPU
- High driver overhead
    - GPU state exchange overhead when solving large amount of drawcalls

<slide class="bg-black-blue slide-top">
:::{.content-left} 

## What Assassins Creed did ?

- Use **mesh cluster** rendering

- In some cases, the culling system is not efficiency. 
   - it could case overdraw, but parts of triangles players can not see it.

<slide class="bg-black-blue slide-top">

:::{.content-left} 

### Cluster
- It is the minimum culling unit. 
   - each **Cluster** contains 128 triangles.
   
### Cluster Group

- it is the simplification unit
   - each **Cluster Group** contains 128 triangles.

### QEM Algorithm

The central idea of the algorithm is to iteratively remove edges in the mesh through a process known as edge contraction which merges the vertices at an edge's endpoints into a new vertex that optimally preserves the original shape of the mesh

<slide class="bg-black-blue slide-top">

:::{.content-left} 
### Culling

#### Frustum Culling

#### Occlusion Culling

<slide class="bg-black-blue slide-top">

:::{.content-left} 
### Visibility Buffer
