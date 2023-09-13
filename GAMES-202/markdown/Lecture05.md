# GAMES202 Lecture 05 - Real-Time Environment Mapping

[GAMES202_Lecture_05 (ucsb.edu)](https://sites.cs.ucsb.edu/~lingqi/teaching/resources/GAMES202_Lecture_05.pdf)

## I. Distance Field Soft Shadows

### (Signed) Distance Function

**Input**: Coordinate

**Output**: The *minimum distance* from that coordinate to the object being described

**Characteristics**:

- **Preserving Boundary**: Blending two SDF results in a moving boundary (rather than a blurred one)

  ![image-20230912183945613](../images/Lecture05-img-1.png)

- **Combing Distance Functions**:

  ![image-20230912184114916](../images/Lecture05-img-2.png)



#### Usages

#### Ray Marching (Sphere Tracing)

Used in ray marching to perform ray-SDF intersections.

![image-20230912184257745](../images/Lecture05-img-3.png)

Each time at a given point $p$, travel $\text{SDF}(p)$ distance.



#### Computing Occlusions (SDF Soft Shadow)

![image-20230912184425904](../images/Lecture05-img-4.png)

The value of $\text{SDF}$ can be used to determine a "safe" angle seen from the eye:

- The smaller the "safe" angle, the less the visibility

![image-20230912184640509](../images/Lecture05-img-5.png)

To compute the angle, use $\min\left\{\frac{k \cdot \text{SDF} (p)}{p - o}, 1.0\right\} $:

- Larger $k$ leads to earlier cutting off of **penumbra**, resulting in **harder** shadows



![image-20230912184947922](../images/Lecture05-img-6.png)

#### Anti-Aliased Characters in RTR

![image-20230912185138801](../images/Lecture05-img-7.png)

[Troika-Three-Text from GitHub](https://github.com/protectwise/troika/tree/master/packages/troika-three-text)



### Pros and Cons

**Pros**:

- Fast
- High quality

**Cons**:

- Needs precomputation
- Needs **heavy** storage:
  - 
- Artifacts:
  - 



## II. Shading from Environment Lightning

### The Split Sum Approximation



