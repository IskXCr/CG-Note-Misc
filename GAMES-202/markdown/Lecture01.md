# GAMES202 Lecture 01 - Introduction and Overview

[GAMES202-Lecture-01.pdf](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES202_Lecture_01.pdf)

## I. Overview

<img src="../images/Lecture01-img-1.png" alt="image-20230727171506180" style="zoom:50%;" />

### What is GAMES202 about?

- A course at **intermediate level**: connecting basic knowledge and research
- **Real-Time** High Quality Rendering
  - **Speed**: More than *30 FPS*, for VR/AR *90 FPS* or above
  - **Interactivity**: Each frame generated on the fly
- Real-Time **High Quality** Rendering
  - **Realism**: Advanced approaches to make rendering more realistic
  - **Dependability**: All-time *correctness (exact or approximate)*, no tolerance to (uncontrollable) failures
- Real-Time High Quality **Rendering**
  - What is rendering? Calculating light which human eyes perceive
    - 3D Scene -> Image



#### Course Topics

**4 Different Parts on Real-Time Rendering**:

- Shadow and Environment Mapping
- Interactive Global Illumination Techniques
- Physically Based Shading
- Real-Time Ray Tracing



**Topics**:

- Shadow and Environment Mapping
- Interactive Global Illumination Techniques
- Precomputed Radiance Transfer
- Real-Time Ray Tracing
- Participating Media Rendering, Image Space Effects, etc.
- Non-Photorealistic Rendering
- Antialiasing and Supersampling
- *Chatting about tech and games*



### What is GAMES202 NOT about?

Overall, not about a specific engine or API, and not about offline rendering.

- 3D modeling or game development, or specific API
- Expensive light transport techniques
- Neural Rendering (*Why?*)
- Specific optimization and high performance computing



### How to study GAMES202?

- Real-time rendering: 
  - *fast & approximate offline rendering + systematic engineering*.
- Fact: 
  - in real-time rendering technologies, the industry is *way ahead of the academia*.
- Practice makes perfect.



## II. Motivation, Evolution and Milestones

### Motivation

<img src="../images/Lecture01-img-2.png" alt="image-20230727172757301" style="zoom: 67%;" />

- As of now, current CG algorithms are able to generate **photorealistic** images
  - Complex geometry, lighting, materials, shadows, ...
  - Computer-generated movies/special effects (*difficult or impossible to tell real from rendered*)
- But **accurate algorithms** (esp. ray tracing) are **very slow**:
  - Therefore, the phrase **offline rendering**
- Proper approximations can lead to **plausible** results but run much faster



### Evolution of Real-Time Rendering

- Interactive 3D graphics pipeline as in OpenGL

  <img src="../images/Lecture01-img-3.png" alt="image-20230727173225137" style="zoom: 67%;" />

  - Earliest SGI machines (Clark 82) to today
  - Most of focus on more geometry, texture mapping
  - Some tweaks for realism (shadow mapping, accum. buffer)

- 20 years ago

  <img src="../images/Lecture01-img-4.png" alt="image-20230727173314093" style="zoom: 58%;" />

  - Interactive 3D geometry with simple texture mapping, fake shadows (OpenGL, DirectX)

- 10~20 years ago

  <img src="../images/Lecture01-img-5.png" alt="image-20230727173446116" style="zoom:55%;" />

  - A giant leap since the emergence of **programmable shaders**
  - Complex environment lighting, real materials (velvet, satin, paints), soft shadows

- **Today**

  <img src="../images/Lecture01-img-6.png" alt="image-20230727173555160" style="zoom:50%;" />

  - Stunning graphics, extended to VR and even movies

- *In the future*

  - <img src="../images/Lecture01-img-7.png" alt="image-20230727173643720" style="zoom:52%;" />



### Technological and Algorithmic Milestones

- Programmable graphics hardware (shader) (**20 years ago**)

  <img src="../images/Lecture01-img-8.png" alt="image-20230727173944607" style="zoom:59%;" />

- Precomputation-based methods (**15 years ago**)

  - Complex visual effects are (partially) **pre-computed**

  - Minimum rendering cost at **runtime**

  - **Relighting**

    - Fix geometry
    - Fix viewpoint
    - Dynamically change lighting

  - **Interactive Ray Tracing** (**8~10 years ago**: CUDA + OptiX)

    - Hardware development allows ray tracing on GPUs at low sampling rates (~1 samples per pixel (SPP))
    - Followed by post-processing to denoise

    <img src="../images/Lecture01-img-9.png" alt="image-20230727174249902" style="zoom:58%;" />

