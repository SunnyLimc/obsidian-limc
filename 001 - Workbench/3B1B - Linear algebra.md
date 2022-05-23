a vector can be anything where there's a sensible notion of adding two vectors and multiplying a vector by a number

in linear algebra perspective, a vector always will be rooted at the origin(原点).

- the addition (sum) : move the second one's tail to first one's tip, and also keep the second one keep the original orientation, and draw a new vector from the tail of the first one to where the tip of the second one. 
	- you can treat vector as a orientation-specified movement in coordinate
	- or an addition on the number line. (treat x and y axes as two separate number line)

 - the scalar-multiplication
	 - treat as scaling a vector with a scalars(标量)
	 - as the same as mentioned above (treat x .. as two separate number line)

- the description of vector depends on an implicit choice of what basis vectors we're using
	- linear combination ($a\vec{v}+b\vec{w}$)
		- fix one (scalars) and alter the other one you will draw a **straight** line
		- alter both can represent every possible vector in the plane.
			- except both of them are initially collinear (happen to line up)
			- both vector zeroed (stuck at the origin)
			- **all possible vectors that you can reach with linear combination called `span` of those 2 vectors**
		- think the vectors as points without an arrow if there is a collection of vector
			- easy to understand with the concept of **`span`** above
		- if there is three-dimension, linear combination will generate a flat that cutting through the origin
		- the coordinate of x and y of one vector was stretch out with a specific number of scalar
	- add an addition vector $a\vec{v}+b\vec{w}+c\vec{u}$ can get the whole three-dimension span
		- if one of vectors located at which plane of `span` create by the other two ones, the `span` of those 3 vectors can only be a specific plane
		- in other words, if a third vector can be formed by scaling and adding other two vectors, then any two vectors can form another vector by scaling and adding, since the relationship of addition between them are fixed and no matter with any dimensions you add.
		- in other other word: if three vectors are in the same line, the `span` of them is a line.
		- if a vector that can not be form from any another (`one`, `two` .. `dimension - 1`) vector(s), the dimension of `span` will increased by 1. This relationship named with the terminology: `Linearly independent (线性无关)`  officially written as $\vec{u} \neq b\vec{w}+c\vec{u}$ in three dimension and $\vec{u} \neq a\vec{w}$ in the case of two dimension
		- a terminology to describe the situation that a redundant vector (two vector are in the same line) exist is: `Linearly dependent (线性相关)`
		- the **`basis`** is a set of linearly independent vectors that can span the full space.

- linear transformation
	- Principles
		- Lines remain lines
		- Origin remains fixed
	- Grid lines **remain parallel** and **evenly spaced**
	- You needn't know the transformation itself, for what bolded above.
		- the whole thing you need to know the value of $\hat{i}$ and $\hat{j}$ that have been transformed.
		
		- formula ![[Pasted image 20220520195038.png | 400]]
		- a transformation of anticlockwise rotate 90 degree $$ \begin{bmatrix}
	0 & -1 \\
	1 & 0 
	\end{bmatrix}   
	\begin{bmatrix}
	x \\
	y 
	   \end{bmatrix} $$
		- sheer: fix $\hat{i}$ and 45 degree clockwise rotate $\hat{j}$ 		   $$ \begin{bmatrix}
	1 & 1 \\
	0 & 1 
	\end{bmatrix}   
	\begin{bmatrix}
	x \\
	y 
	   \end{bmatrix} $$
		- if two transformed `i` and `j` in `matrix` are `Linearly dependent`, the `span` that transformation generate is a one-dimension line. It technically named `Linearly dependent columns`.

- composite transformations
	- written provisions
		- read from right to left, since the closest function symbol always at the left of $x$
	- apply $\hat{i}$ to matrix first and then apply $\hat{j}$

In fact, the transformation (matrix) is by moving and scaling the axis, so it needs the same vectors number as the dimensions to record the result of transformation.

- the `determinant` of a transformation is the special scaling factor
	- a factor that can be applied to **any** area transformation.
	- if `det` is zero, means the result of transformation has squished to a smaller dimension
		- at least two axes are located at a same line
	- a negative `det` means to invert the orientation (in 3D you can give the judgement by left-handed rule)
		- you can perfectly get two separate representing of 3D shape by rotating your left hand with left-handed rule
	- you do not need to comprehend why $ad-bc$ can calculate the scalar of 2D 
		- just a image to explain why![[Pasted image 20220522205434.png | 500]]
	- an interesting fact: $\det(M_1M_2)=\det(M_1)\det(M_2)$
		- **scaling is just a property of transformation**, so whatever scaling follow by transforming ($\det(M_1M_2)$), or just scaling ($\det(M_1)\det(M_2)$), both it represent the same thing -- do $M_1$ transform from the basic $\hat{i}\hat{j}\hat{k}$ follow by $M_2$

- we can get origin vector by undo transformation, the pattern of it called `inverse matrix`
	- it's obviously that you **can't undo** a transformation which leads to dimension squishing
		- $A^{-1}A\ \vec{x}=A^{-1}\ \vec{v}$
		- but sometimes $\vec{v}$ come across to the same dimension as $\vec{x}$, solution is exist
- we describe the dimension of spanning the columns of matrix as `column space`
- you may noticed that if it's a non-full-rank transformation, dimensions are squished to a particular point. Vectors that lands on the $\vec{v}$ (if $\vec{v}$ is zero, it terminology called `null space`), **it gives you all of the possible solutions to the equation**.
	- an important property: if a function (take one input and give one output) is **`linear`**, give a series of points that evenly spaced, they will **evenly spaced yet** after transformation.
	- this vector is still full-rank, since it's column space equals to the span of columns - the plane - it just map a 2D plane to 3D space - with 2 input (basis) vectors
$$ \begin{bmatrix}
	2 & 0  \\
	-1 & 1  \\
	-2 & 1
	\end{bmatrix}   
	$$
	- mapping from 3D to 2D, each vector of the 3 is the orientation of axis
$$ \begin{bmatrix}
	2 & 0 & 3 \\
	-1 & 1 & 1 \\
	\end{bmatrix}   
	$$

- dot product
	- sequence irrelevant