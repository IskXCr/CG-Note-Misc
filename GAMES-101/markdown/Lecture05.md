# GAMES101 Lecture 05 - Rasterization 1 (Triangles)

[GAMES101_Lecture_05.pdf](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES101_Lecture_05.pdf)

## I. Transformation Cont.

View `Lecture04.md`.



## II. Overview

- **Barycentric Interpolation**
  - *Will be discussed in the Geometry part!*

- **Rasterization**
  - **Why using triangles?**
    - Guaranteed to be planar
    - Well-defined interior
    - Well-defined method for interpolating values at vertices over triangle (barycentric interpolation)
  - Edge Cases: *typically implementation-dependent.*
  
- **Bounding Box**: Quickly test intersections
  - Axis-Aligned Bounding Box (AABB)

- **Incremental Triangle Rasterization**
  - For thin triangles whose longer axis is tilted, examine *one line each time*.

- Observe Half-Tone Pattern