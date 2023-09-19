# GAMES202 Lecture 09 - Real-Time Global Illumination (Screen Space Cont.)

[GAMES202_Lecture_09 (ucsb.edu)](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES202_Lecture_09.pdf)

## I. Real-Time Global Illumination (Screen Space Cont.)

### Screen Space Directional Occlusion (SSDO)

- An **improvement over SSAO**
- Considering (more) actual indirect illumination

#### Key Idea

- SSDO exploits the rendered direct illumination

  - Not from an RSM, but from the camera

  ![image-20230918213129231](../images/Lecture09-img-1.png)

- Similar to **path tracing**:

  - At shading point $p$, cast a random ray:
    - If it does not hit an obstacle, then direct illumination
    - If it hits one, then indirect illumination

  ![image-20230918213334202](../images/Lecture09-img-2.png)

#### Comparison with SSAO

![image-20230918213427733](../images/Lecture09-img-3.png)

#### Theory

Consider unoccluded and occluded directions **separately**:
$$
\begin{equation} \tag{Direct illumination}
L_o^{\text{dir}} (p, \omega_o)
=
\int_{\Omega^+, V=1} L_i^{\text{dir}} (p, \omega_i) f_r (p, \omega_i, \omega_o) \cos \theta_i \dd{\omega_i}
\end{equation}
$$

$$
\begin{equation} \tag{Indirect illumination}
L_o^{\text{indir}} (p, \omega_o)
=
\int_{\Omega^+, V=0} L_i^{\text{indir}} (p, \omega_i) f_r (p, \omega_i, \omega_o) \cos \theta_i \dd{\omega_i}
\end{equation}
$$

- Indirect illumination from a pixel (patch) is derived in last lecture.

Similar to HBAO, in that they test samples' depths in local **hemispheres**:

![image-20230918213943134](../images/Lecture09-img-4.png)

#### Results

Quality closer to offline rendering.

![image-20230918214031417](../images/Lecture09-img-5.png)

#### Issues

- GI in a **short** range: 

  ![image-20230918214129702](../images/Lecture09-img-6.png)

- **Visibility**:

- **Limitation from screen space**: Missing information from **unseen** surfaces



### Screen Space Reflection (SSR)

- What is SSR?
  - One way to introduce Global Illumination in RTR
  - Perform ray tracing
  - Does not require 3D primitives (**Screen Space**)

- Two fundamental tasks of SSR
  - Intersection: between **any** ray and the scene
  - Shading: contribution from intersected pixels to the shading point

![image-20230919161743190](../images/Lecture09-img-7.png)

![image-20230919161850588](../images/Lecture09-img-8.png)

![image-20230919161927182](../images/Lecture09-img-9.png)



#### Basic Algorithm

![image-20230919162617681](../images/Lecture09-img-14.png)

- For each **fragment**:
  - Compute a reflection ray
  - Trace along ray direction (using depth buffer)
  - Use color of intersection point as reflection color

| Parameters                  | Result                                                     |
| --------------------------- | ---------------------------------------------------------- |
| High Smoothness             | ![img](../images/Lecture09-img-10.png)                     |
| Medium Smoothness           | ![img](../images/Lecture09-img-11.png)                     |
| Medium Smoothness + Normals | ![img](../images/Lecture09-img-12.png)                     |
| Variable Smoothness         | ![image-20230919162547573](../images/Lecture09-img-13.png) |



#### Linear Raymarch

Goal: Find intersection point

- At each step, check depth value
- Quality depends on step size
- Can be refined



