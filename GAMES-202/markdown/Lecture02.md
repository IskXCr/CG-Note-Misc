# GAMES202 Lecture 02 - Recap of CG Basics

[GAMES202_Lecture_02 (ucsb.edu)](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES202_Lecture_02.pdf)

## Outline

- Basic GPU Hardware Pipeline
- OpenGL
- OpenGL Shading Language (**GLSL**)
- The Rendering Equation
- Calculus



## I. Basic GPU Hardware Pipeline

### Graphics Pipeline

<img src="../images/Lecture02-img-1.png" alt="image-20230728151130552" style="zoom:50%;" />

- **MVP Transformation**
- **Sampling Triangle Coverage**
- **Z-Buffer Visibility Tests**
- **Shading**
  - Blinn-Phong Reflectance Model
- **Texture Mapping & Interpolation**



## II. OpenGL

OpenGL is a set of APIs that call the GPU pipeline from CPU

- Therefore, the caller language does not matter.

- Cross-platform
- Alternatives: DirectX, Vulkan, etc.



**Cons**

- Fragmented: lots of different versions
- C style, not easy to use
- Cannot debug(?)



**Understanding**

- One-to-one mapping to our software rasterizer in GAMES101.



### Usage and Analogy

Important Analogy: Oil Painting

1. Place objects/models
2. Set position of an easel
3. Attach a canvas to the easel
4. Paint to the canvas
5. (Attach other canvases to the easel and continue painting)
6. (Use previous paintings for reference)



In OpenGL (one-to-one):

1. (*Place objects/models*) Model specification and transformation:

   - User specifies an object's vertices, normals, texture coordinates and send them to GPU as a Vertex Buffer Object (**VBO**)
     - Very similar to `.obj` files
   - Use OpenGL functions to obtain matrices
     - e.g., `glTranslate`, `glMultMatrix`, etc.
     - No need to write anything on our own.

2. (*Set position of an easel*) View transformation, Creating and Using a Framebuffer:

   - Set camera (the viewing transformation matrix) by simply calling e.g., `gluPerspective`:

     ```C
     void gluPerspective(GLdouble fovy, GLdouble aspect, GLdouble zNear, GLdouble zFar)
     ```

3. (*Attach a canvas to the easel*) Specify a framebuffer to use. One **Pass** in OpenGL

   - Specify one or more textures as output (shading, depth, etc.)
   - Render (fragment shader specifies the content on each texture)

4. *(Paint to the canvas)* For each vertex/primitive, ..., in parallel

   - For example, how to perform shading.

   - For each **vertex** in parallel:

     - OpenGL calls user-specified vertex shader: transformation matrices, other ops.

   - For each primitive, OpenGL rasterizes
     - Generates a *fragment* for each pixel the fragment covers


   -  For each **fragment** in parallel:
     - OpenGL calls user-specified fragment shader: Shading and lighting calculations
     - OpenGL handles z-buffer depth test unless overwritten

5. (*Attach other canvases to the easel and continue painting*)

   Same as 3.

6. (*Use previous paintings for reference*) Multiple passes

   - Use your previous painting for reference



**Summary**: in each pass

- Specify objects, camera, MVP, etc.
- Specify framebuffer and input/output textures
- Specify vertex/fragment shaders
- (When you have everything specified on the GPU) Render!



## III. OpenGL Shading Language (GLSL)

### Shading Languages

- Vertex/Fragment shading described by small program
- Written in language similar to **C** but with restrictions
- Long history. Cook's paper on Shade Trees, Renderman for offline renderings
  - In ancient times: **assembly** on GPUs!
  - Stanford Real-Time Shading Language, work at SGI
  - Still long ago: Cg from NVIDIA
  - HLSL in DirectX (vertex + pixel)
  - **GLSL** in OpenGL (vertex + fragment)

### Shader Setup

- Initializing
  - Create shader (Vertex and Fragment)
  - Compile shader
  - Attach shader to program
  - Link program
  - Use program
- Shader source is just sequence of strings.
- Similar to compiling a normal program.



### Debugging Shaders

- Years ago: NVIDIA Nsight with Visual Studio
  - Multiple GPUs are needed for debugging GLSL
  - Had to run in software simulation mode in HLSL
- Now
  - Nsight Graphics (cross-platform, NVIDIA GPUs only)
  - RenderDoc (cross-platform, no limitations on GPUs)

- Personal Advice (from Prof.)
  - Print it out by showing values as **colors**



## IV. The Rendering Equation

In **real-time rendering** (RTR)
$$
\begin{equation} \label{rendeq} \tag{1}

L_o (\text{p}, \omega_o) = 

L_e (\text{p}, \omega_o) + 

\int_{\Omega_{+}} L_i (\text{p}, \omega_i) f_r(\text{p}, \omega_i, \omega_o)

(\textbf{n} \cdot \omega_i) V(\text{p}, \omega_i) \dd{\omega_i}

\end{equation}
$$

- **Visibility** $ V(\text{p}, \omega_i)$ is often explicitly considered
- BRDF is often considered together with the cosine term



### Environment Lighting

- Represent incident lighting from all directions
  - Usually represented as a cube map or a sphere map (texture)
  - A new representation will be introduced in this course

