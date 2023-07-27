# GAMES101 Lecture 04 - Transformation Cont.

[GAMES101_Lecture_04.pdf](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES101_Lecture_04.pdf)

## I. Rodrigues' Rotation Formula

Rotation by angle $\alpha$ around axis $\textbf{n}$

$$
R(\textbf{n}, \alpha) = \cos(\alpha)\textbf{I} + (1 - \cos(\alpha))\textbf{n}\textbf{n}^T + \sin(\alpha)
\underbrace{\begin{bmatrix}
0 & -n_z & n_y \\\\
n_z & 0 & -n_x \\\\
-n_y & n_x & 0 
\end{bmatrix}}_{\textbf{N}}
$$


## II. MVP Transformation

### MVP Transformation

*Model-Viewing-Projection Transformation.*

- **View/Camera Transformation**: Converts points in canonical coordinates (or *world coordinates*) to *camera coordinates* or places them in *camera space*.
  - *Camera Space - Eye Space*
- **Projection Transformation**: Moves points from camera space to the *canonical view volume*.
  - Sometimes called *Viewing Transformation*
  - *Canonical View Volume - Clip Space or Normalized Device Coordinates*
- **Viewport Transformation**: Maps the canonical view volume to *screen space*.
  - *Screen Space - Pixel Coordinates*



#### View/Camera Transformation

Let the following attributes be that of the camera:

- $\vec{e}$: the position of the camera
- $\hat{g}$: the gaze direction
- $\hat{t}$: up direction, assuming perpendicular to the gaze direction

The target of the view transformation is to transform the coordinate system such that:

- the camera is at the origin, and
- its up direction is the new $y$ axis and it looks at the direction of $-z$.

When deducting the matrix for view transformation, consider the *inverse* rotation which rotates the $x$ axis to $\hat{g} \times \hat{t}$, the $y$ axis to $\hat{t}$ and the $z$ axis to $-\hat{g}$.



#### Projection Transformation

##### Orthographic projection:

The purpose of this projection is to transform the selected volume such that it then centers at origin and has a normalized size of $[-1, 1]^3$.

$$
M_{\text{ortho}}=
\begin{bmatrix}
\frac{2}{r - l} & 0 & 0 & 0 \\\\
0 & \frac{2}{t - b} & 0 & 0 \\\\
0 & 0 & \frac{2}{n - f} & 0 \\\\
0 & 0 & 0 & 1 
\end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 & -\frac{r + l}{2} \\\\
0 & 1 & 0 & -\frac{t + b}{2} \\\\
0 & 0 & 1 & -\frac{n + f}{2} \\\\
0 & 0 & 0 & 1 
\end{bmatrix}
$$

$$
M_{\text{ortho}}=
\begin{bmatrix}
\frac{2}{r - l} & 0 & 0 & -\frac{r + l}{r - l} \\\\
0 & \frac{2}{t - b} & 0 & -\frac{t + b}{t - b} \\\\
0 & 0 & \frac{2}{n - f} & -\frac{n + f}{n - f} \\\\
0 & 0 & 0 & 1 
\end{bmatrix}
$$

*Note that $z_n > z_f$ by convention.*

If such an orthographic projection happens after applying a perspective projection, then the matrix can be simplified to:

$$
M_{\text{ortho}}=
\begin{bmatrix}
\frac{2}{r - l} & 0 & 0 & 0 \\\\
0 & \frac{2}{t - b} & 0 & 0 \\\\
0 & 0 & \frac{2}{n - f} & -\frac{n + f}{n - f} \\\\
0 & 0 & 0 & 1 
\end{bmatrix}
$$



##### Perspective projection:

$$
M_{\text{persp}} = M_{\text{ortho}}M_{\text{persp$\to$ortho}}=
\begin{bmatrix}
\frac{2n}{r - l} & 0 & -\frac{r + l}{r - l} & 0 \\\\
0 & \frac{2n}{t - b} & -\frac{t + b}{t - b} & 0 \\\\
0 & 0 & \frac{n + f}{n - f} & -\frac{2nf}{n - f} \\\\
0 & 0 & 1 & 0 
\end{bmatrix}
$$

where

$$
M_{\text{persp$\to$ortho}}=
\begin{bmatrix}
n & 0 & 0 & 0 \\\\
0 & n & 0 & 0 \\\\
0 & 0 & n + f & -nf \\\\
0 & 0 & 1 & 0 
\end{bmatrix}
$$
