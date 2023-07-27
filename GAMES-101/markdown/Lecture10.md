# GAMES101 Lecture 10 - Geometry 1 (Introduction)

[GAMES101_Lecture_10.pdf](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES101_Lecture_10.pdf)

## I. Application of Textures

*Refer to* `Lecture09.md`.



## II. Introduction to Geometry

### Examples of Geometry

...

### Ways to Represent Geometry

- **Implicit**: Points satisfy some specified relationship. Example:

  $$
  f(x, y, z) = 0
  $$
  

  - **Pros**:

    - Compact description
    - Certain queries easy (inside object, distance to surface)
    - Good for ray-to-surface intersection
    - For simple shapes, exact description / no sampling error
    - Easy to handle changes in topology

  - **Cons**:

    - Difficult to model complex shapes

  - **Examples**:

    - *Algebraic Surfaces*

      $$
      x^2 + y^2 + z^2 = 1
      $$

    - *Constructive Solid Geometry*: Combine implicit geometry via Boolean operations

    - *Level Set Methods*: Store a **grid** of values approximating a function. Surface is found where interpolated values equal zero 

    - *Distance Functions*: Given minimum distance (could be **signed** distance) from anywhere to object

      - Blend a moving boundary
      - Blend any two distance functions

    - *Fractals*: Exhibit self-similarity, detail at all scales



- **Explicit**: All points are **given directly** or via **parameter mapping**. Example:
  
  $$
  f:\mathbb{R}^2 \to \mathbb{R}^3;(u,v) \to (x,y,z)
  $$

  - Easy to sample

  - Hard to test inside/outside

  - **Examples**:*

    - *Point Cloud*

      - Easiest representation
      - Useful for **large** datasets
      - Often converted into polygon mesh
      - Difficult to draw in under-sampled regions

    - *Polygon Meshes*

      - Store vertices & polygons (often *triangles* or *quads*)

      - Easier to do processing / simulation, adaptive sampling

      - More complicated data structures

      - Perhaps the most common representation in graphics

      - **Format Example**:

        - The Wavefront Object File (`.obj`) Format

          Text file specifying vertices, normals ,texture coordinates and their connectivity (by *specifying for example the triangular face, with normals and texture coordinates on three vertices*)

    - *Bezier Surfaces*

    - *Polygon Mesh*

    - *Subdivision Surfaces*

    - *NURBS*

  - ...
