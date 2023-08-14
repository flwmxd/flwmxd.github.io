---
title: Fake-Nanite
date: 2023-08-01 23:00:00
tags: Geometric Processing
index_img : /images/nanite/virtual_geometry.png
description: Virtual Geometry build-in MapleEngine. Fake Nanite implementation.
sticky: 100
---

# Introduction

Implement Nanite in my personal Game Engine( it is an experimental function now).  
the goal is to render more than 10 billion faces.

<p align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/79aaFzgOso0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</p>

## What we need in next generation model Rendering?

To achieve a lifelike cinematic cutscene, character models need to be incredibly detailed; for a sufficiently flexible and rich open world, both the map size and the number of objects need to grow exponentially. Both of these factors significantly increase the demands on scene intricacy and complexity: the quantity of scene objects needs to be high, and each model needs to be finely detailed.

There are typically two bottlenecks when it comes to rendering complex scenes: (1) the CPU-side validation and CPU-GPU communication overhead caused by each Draw Call; (2) the overdraw resulting from imprecise occlusion culling, leading to wastage of GPU computational resources. In recent years, optimizations in rendering technology have often revolved around these two challenges, leading to some industry-wide technical consensus.

- the one is next generation graphics API (Vulkan/DX12)
- another one is **GPU Driven Pipeline**

Thanks to the increasingly widespread adoption of the GPU Driven Pipeline in games, further subdividing the vertex data of models into Clusters (or Meshlets) has become a best practice for optimizing complex scenes. This approach allows each cluster to better align with the cache size of the Vertex Processing stage. Various culling techniques like Frustum Culling, Occlusion Culling, and Backface Culling are then applied on a per-cluster basis, effectively optimizing intricate scenes.

## Is that enough ?
At this point, the issues related to the number of models, triangle vertices, and faces have been greatly optimized and improved. However, high-poly models and pixel-level small triangles introduce new challenges to the rendering pipeline: the pressures of rasterization and overdraw.

## Is there a chance for soft rasterization to defeat hard rasterization?

In simple terms, traditional hardware design for rasterization initially envisioned input triangles that are much larger than a single pixel. Based on this idea, the rasterization process in hardware is typically hierarchical. Taking the example of the rasterizer in an NVIDIA graphics card, a triangle usually goes through two stages of rasterization: Coarse Raster and Fine Raster.

### Coarse Raster
In the Coarse Raster stage, a triangle is taken as input and divided into blocks of 8x8 pixels each. This means the triangle is rasterized into several of these blocks, effectively performing a rough rasterization on a framebuffer that's 1/8th the size of the original framebuffer. In this stage, using a low-resolution Z-buffer, hidden blocks that are occluded are culled out—referred to as Z Culling. After Coarse Raster, the blocks that pass through Z Culling are sent to the next stage, Fine Raster, where the final pixels for shading calculations are generated.

### Fine Raster
In the Fine Raster stage, we encounter the concept of Early Z. Due to the computation required for mip-map sampling, it's necessary to know information about neighboring pixels for each pixel and to use the differences in sampled UV coordinates as the basis for mip-map level calculations. Consequently, the output of the Fine Raster stage isn't individual pixels, but rather 2x2 mini pixel blocks known as Pixel Quads.

For triangles that are close to the size of a single pixel, the inefficiency of hardware rasterization becomes evident. Firstly, the Coarse Raster stage is almost useless in such cases, as these triangles are typically smaller than 8x8 pixels. For elongated triangles, this situation is even worse since a triangle might span multiple blocks, and Coarse Raster not only fails to cull these blocks but also adds extra computational load. Additionally, for large triangles, the Fine Raster stage based on Pixel Quads only generates a few useless pixels along the edges compared to the overall triangle area. However, for small triangles, Pixel Quads can generate up to four times the number of pixels as the triangle's area, and these pixels are also included in the execution of the pixel shader, significantly reducing the number of effective pixels in a warp.

Based on the reasons mentioned above, under the specific premise of small triangles at the pixel level, software rasterization (based on Compute Shader) indeed has the potential to outperform hardware rasterization. 

# Deferred rendering

Generally speaking, deferred rendering pipelines require a set of Render Targets called G-Buffer, where all the material information needed for lighting calculations is stored. In today's AAA games, the variety of materials often becomes complex and diverse, leading to a gradual increase in the information that needs to be stored in the G-Buffer.

# Visibility Buffer

For scenes with high overdraw, the read-write bandwidth generated by G-Buffer rendering often becomes a performance bottleneck. As a result, a new rendering pipeline called the **Visibility Buffer** has been proposed. Algorithms based on the Visibility Buffer approach no longer generate a bulky G-Buffer separately; instead, they use a lower bandwidth-consuming Visibility Buffer as a substitute. The Visibility Buffer typically requires the following information:

- (1) InstanceID, indicating which Instance the current pixel belongs to (16~24 bits);
- (2) PrimitiveID, indicating which triangle of the Instance the current pixel belongs to (8~16 bits);
- (3) Barycentric Coord, representing the position of the current pixel within the triangle using barycentric coordinates (16 bits);
- (4) Depth Buffer, representing the depth of the current pixel (16~24 bits);
- (5) MaterialID, indicating which material the current pixel belongs to (8~16 bits).

In the above approach, we only need to store approximately 8~12 Bytes/Pixel to represent the material information of all geometry in the scene. Simultaneously, we need to maintain a global vertex data and material texture table, containing the vertex data for all geometry in the current frame, as well as material parameters and textures. During the lighting and shading phase, it's only necessary to index relevant triangle information from the global Vertex Buffer based on InstanceID and PrimitiveID. Moreover, using the barycentric coordinates of the pixel, interpolate the per-pixel information from the Vertex Buffer, including UV coordinates, Tangent Space, and other attributes. Furthermore, use the MaterialID to index relevant material information, execute texture sampling operations, and input these into the lighting calculation stage to ultimately complete the shading process. Sometimes, this kind of method is also referred to as Deferred Texturing.

Intuitively, the Visibility Buffer reduces the storage bandwidth required for shading information (from G-Buffer to Visibility Buffer). Additionally, it defers the reading of geometry and texture information related to lighting calculations to the shading phase. Consequently, pixels that are not visible on the screen no longer need to read this data; they only require vertex position information. Due to these two reasons, in high-resolution complex scenes, the bandwidth overhead of the Visibility Buffer is significantly lower compared to traditional G-Buffer approaches.

