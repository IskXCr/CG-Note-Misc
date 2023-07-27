# GAMES101 Lecture 06 - Rasterization 2 (Antialiasing and Z-Buffering)

[GAMES101_Lecture_06.pdf](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES101_Lecture_06.pdf)

## I. Antialiasing - Sampling Artifacts

Signals are *changing too fast* (high frequency) but *sampled too slowly*.

- **Nyquist-Shannon Sampling Theorem**: If a function $x(t)$ contains no frequencies higher than $B$ hertz, then it can be completely determined from its ordinates at a sequence of points spaced less than $\frac{1}{2B}$ seconds apart.
  - ***Higher frequencies need faster sampling***.

- **Aliases**: Two frequencies that are indistinguishable at a given sampling rate are called "aliases".



- **Convolution Theorem**: Convolution in the spatial domain is equal to multiplication in the frequency domain, and vice versa.



- **Sampling Artifact**: Caused by sparse sampling.



### Antialiasing

- Increase sampling rate:

  - Higher res. displays, sensors, framebuffers...
  - Costly and may need very high solution

- **Antialiasing**

  *Make Fourier contents "narrower" before repeating (low-pass filter).*

  - **MSAA** - Multi-Sample Anti-Aliasing: Approximate the effect of the 1-pixel box filter by sampling multiple locations within a pixel and averaging their values.
  -  **FXAA** - Fast Approximate AA
  - **TAA** - Temporal AA

- **Super Resolution / Super Sampling**

  - **DLSS** - Deep Learning Super Sampling



## II. Z-Buffering

 *Store current min $z$ for each sample (pixel).*

- ***Transparent** objects cannot be directly processed .*
