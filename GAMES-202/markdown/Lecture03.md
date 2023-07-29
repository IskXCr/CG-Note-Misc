# GAMES202 Lecture 03 - Real-Time Shadows 1

[GAMES202_Lecture_03 (ucsb.edu)](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES202_Lecture_03.pdf)

<img src="../images/Lecture03-img-16.png" alt="image-20230728211702865" style="zoom:33%;" />

## I. Shadow Mapping

### Recap

Shadow Mapping is:

- A **2-Pass** Algorithm
  - The light pass generates the shadow mapping, or SM
  - The camera pass uses the SM
- An **Image-Space** Algorithm
  - Pro: No required knowledge of scene's geometry
  - Con: Causing self-occlusion and aliasing issues
- A well-known shade rendering technique
  - Basic shadowing technique even for early offline renderings, e.g., Toy Story



| Pass 1                                                       | Pass 2A                                                   | Pass 2B                                                   |
| ------------------------------------------------------------ | --------------------------------------------------------- | --------------------------------------------------------- |
| <img src="../images/Lecture03-img-1.png" alt="image-20230728161919985"  /> | ![image-20230728161944563](../images/Lecture03-img-2.png) | ![image-20230728161959964](../images/Lecture03-img-3.png) |

- Pass 1
  - Output a "depth texture" from the light source
- Pass 2
  - Pass **2A**: Render a standard image from the eye
  - Pass **2B**: Project visible points in eye view back to light source. Test the depth.
    - If the depth does not match the previous one, then the point being rendered is *blocked* from the light source.

<img src="../images/Lecture03-img-4.png" alt="image-20230728162145321" style="zoom:50%;" />

- Notice how specular highlights never appear in shadows
- Notice how curved surfaces cast shadows on each other





| Depth View                                                   | Eye's View (Depth)                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="../images/Lecture03-img-5.png" alt="image-20230728163359092" style="zoom: 50%;" /> | <img src="../images/Lecture03-img-6.png" alt="image-20230728163419528" style="zoom: 50%;" /> |

- Depth View - Viewed from light source
- Eye's View - Viewed from camera, projecting the depth map onto the eye's view.



### Issues in Shadow Mapping

#### Self Occlusion

<img src="../images/Lecture03-img-7.png" alt="image-20230728164647761" style="zoom:50%;" />

- Adding a (variable) bias to reduce self occlusion
  - But introducing *detached shadow*

<img src="../images/Lecture03-img-8.png" alt="image-20230728164740245" style="zoom:50%;" />

- Second-Depth Shadow Mapping

  - Using the midpoint between first and second depths in SM
  - Unfortunately, requires objects to be watertight:
    - ?
  - And the overhead may not worth it.

  <img src="../images/Lecture03-img-9.png" alt="image-20230728165033670" style="zoom:50%;" />



#### Aliasing

<img src="../images/Lecture03-img-10.png" alt="image-20230728165118670" style="zoom: 67%;" />



## II. The Math behind Shadow Mapping

### Approximate the Rendering Equation

In real-time rendering, we care about **fast and accurate approximation**. (Approximately equal)

An important approximation throughout RTR:
$$
\int_\Omega f(x) g(x) \dd{x} \approx \frac{\int_\Omega f(x) \dd{x}}{\int_\Omega \dd{x}} \cdot \int_\Omega g(x) \dd{x}
$$

- Approximate the result of integration by...
  - Why: 
- When is it (more) accurate?
  - ...



The rendering equation with term $V(\text{p}, \omega_i)$ for explicit visibility:
$$
\begin{equation} \label{rendeq} \tag{1}

L_o (\text{p}, \omega_o) = 

L_e (\text{p}, \omega_o) + 

\int_{\Omega_{+}} L_i (\text{p}, \omega_i) f_r(\text{p}, \omega_i, \omega_o)

(\textbf{n} \cdot \omega_i) V(\text{p}, \omega_i) \dd{\omega_i}

\end{equation}
$$
can be approximated as
$$
L_o(\text{p}, \omega_o)
\approx
\frac{
	\int_{\Omega+} V(\text{p}, \omega_i) \dd{\omega_i}
} {
	\int_{\Omega+} \dd{\omega_i}
}
\cdot
\int_{\Omega+} L_i (\text{p}, \omega_i) f_r(\text{p}, \omega_i, \omega_o) \cos \theta_i \dd{\omega_i}
$$

- When is it accurate?
  - Small support: Point/directional lighting
  - Smooth integrand: Diffuse BSDF/Constant radiance area lighting



## III. Percentage Closer Soft Shadows

<img src="../images/Lecture03-img-11.png" alt="image-20230728180618799" style="zoom:50%;" />

### Percentage Closer Filtering (PCF)

- Provides anti-aliasing at shadows' **edges**
  - Not for soft shadows (PCSS, which is for soft shadows, will be introduced later)
  - Filtering the results of shadow comparisons
- Why not filter the shadow map?
  - Texture filtering just averages color components:
    - You'll get **blurred** shadow map first
  - Averaging depth values and comparing then will still lead to a binary visibility.

#### Rough Solution

- Perform multiple (e.g., $7 \times 7$) depth comparisons for each fragment
- Then, averages the results of comparisons
- For example, 
  - For point $P$ on the floor, 
    1. Compare its depth $D$ with all pixels in the red box, e.g., $3 \times 3$
    2. Get the compared results, e.g., $\begin{bmatrix} 1 & 0 & 1 \\ 1 & 0 & 1 \\ 1 & 1 & 0 \end{bmatrix}$
    3. Take the average to get visibility, e.g., $0.667$

<img src="../images/Lecture03-img-12.png" alt="image-20230728181322513" style="zoom: 50%;" />

<img src="../images/Lecture03-img-13.png" alt="image-20230728181503347" style="zoom:50%;" />

**Problems**:

- Does filtering size matter?
  - Smaller -> Sharper
  - Larger -> Softer

- Can we use PCF to achieve soft shadow effects?
  - ...
- Key thoughts:
  - From hard shadows to soft shadows
  - What's the **correct size** to filter?
  - Is it uniform?



### Complete PCSS

<img src="../images/Lecture03-img-15.png" alt="image-20230728182327005" style="zoom:50%;" />

- Sharper as the tip gets closer to the plane. Softer in the other direction.

<img src="../images/Lecture03-img-14.png" alt="image-20230728182053622" style="zoom:33%;" />

- $ \text{Filter Size} \leftrightarrow \text{Blocker Distance}$

  - More accurately, **relative average** projected blocker depth.

  $$
  w_{\text{Penumbra}} = (d_{\text{Receiver}} - d_{\text{Blocker}}) \cdot w_{\text{Light}} / d_{\text{Blocker}}
  $$

#### The complete algorithm of PCSS

1. Blocker Search: Get the average blocker depth in a certain region
2. Penumbra Estimation: Use the average blocker depth to determine filter size
3. Percentage Closer Filtering



- Which region to perform blocker search?

  - Can be set constant (e.g., $5 \times 5$), but can be better with heuristics

  - Depends on the size of light, and distance between receiver and the light

