a vector can be anything where there's a sensible notion of adding two vectors and multiplying a vector by a number

in linear algebra perspective, a vector always will be rooted at the origin(原点)].

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
		- in other words, if the third vector can be formed by scaling and adding other two vectors, then any two vectors can form another vector by scaling and adding, since the relationship of addition between them are fixed and no matter with any dimensions you add.
		- in other other word: if three vectors are in the same line, the `span` of them is a line.
		- if a vector that can not be form from any another (`one`, `two` .. `dimension - 1`) vector(s), the dimension of `span` will increased by 1. This relationship named with the terminology: `Linearly independent (线性无关)`  officially written as $\vec{u} \neq b\vec{w}+c\vec{u}$ in three dimension and $\vec{u} \neq a\vec{w}$ in the case of two dimension ^33b084
		- a terminology to describe the situation that a redundant vector (two vector are in the same line) exist is: `Linearly dependent (线性相关)`
		- the **`basis`** is a set of linearly independent vectors that can span the full space.

- linear **transformation**
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

- the `determinant` of a transformation is the **a special scaling factor**
	- a factor that can be applied to **any** area transformation.
	- if `det` is zero, means the result of transformation has squished to a smaller dimension
		- at least two axes are located at a same line
	- a negative `det` means to **invert the orientation** (in 3D you can give the judgement by left-handed rule)
		- you can perfectly get two separate representing of 3D shape by rotating your left hand
	- you do not need to comprehend why $ad-bc$ can calculate the scalar of 2D
		- just a image to explain why![[Pasted image 20220522205434.png | 500]]
	- an interesting fact: $\det(M_1M_2)=\det(M_1)\det(M_2)$
		- **scaling is just a property of transformation**, so whatever scaling follow by transforming ($\det(M_1M_2)$), or just scaling ($\det(M_1)\det(M_2)$), both it represent the same thing -- do $M_1$ transform from the basic $\hat{i}\hat{j}\hat{k}$ follow by $M_2$

- we can get origin vector by undo transformation, the pattern of it called `inverse matrix`
	- it's obviously that you **can't undo** a transformation which leads to dimension squishing
		- $A^{-1}A\ \vec{x}=A^{-1}\ \vec{v}$
		- but sometimes $\vec{v}$ come across to the same dimension as $\vec{x}$, solution is exist
		- apply $A^{-1}$ to scheme $\vec{v}$ to realize the transformation from $\vec{v}$ to $A^{-1}$
- we describe the dimension of spanning the columns of matrix as `column space`

- the rank of a matrix is the maximum number of linearly independent column vectors(or row vectors)
- you may noticed that if it's a non-full-rank transformation, dimensions are **squished to a** particular point. Vectors that lands on the $\vec{v}$ (if $\vec{v}$ is zero, it terminology called `null space`), **it gives you all of the possible solutions to the equation**.
	- an important property: if a function (take one input and give one output) is **`linear`**, give a series of points that evenly spaced, they will **evenly spaced yet** after transformation.
	- this vector is still full-rank, since it's column space equals to the span of columns - the plane - it just map a 2D plane to 3D space - with 2 input (basis) vectors
$$ \begin{bmatrix}
	2 & 0  \\
	-1 & 1  \\
	-2 & 1
	\end{bmatrix}   
	$$
- mapping from 3D to 2D, each vector of the 3 is the orientation of axis
	- basically a 2D-plane with a particular offset and orientation
	$$ \begin{bmatrix}
	2 & 0 & 3 \\
	-1 & 1 & 1 \\
	\end{bmatrix}   
$$
	- **Although it does not satisfied the high-dimension condition, but still contains meaningfulness in high-dimension from the low-dimension viewing**
	$$\begin{bmatrix}
	1\\
	2\\
	\end{bmatrix}
	$$
- dot product
	- sequence irrelevant
	- we all know that $[u_x, u_y]$ is the transformation result of $[\hat{i}, \hat{j}]$ AKA $(1, 1)$, and in the case we want to calculate $u_x$, we should map $\hat{i}$ to the axis, the same true to $\hat{j}$ and then you can get $[u_x, u_y]$. If $\hat{u}$ equals to 1, remap $\hat{u}$ to axis x and y, **You will wondrously finding that remapping $u_x$ and $u_y$ from $\hat{u}$ to $[x, y]$ are the same as to $[u_x, u_y]$.**
	- so if you know the values of $[u_x, u_y]$, you can directly know the point $(x ,y)$ from remapping $\hat{u}$ $\begin{bmatrix} x \\ y \\ \end{bmatrix}$ to current $(x, y)$ axes . we call it **`duality`**
		- $v_x, v_y$ is technologically separate
		- the result can be expressed numerically because it's one dimension
		- the axis can be put into the space since it's one-dimension and can be **orientated** represent in space 
		- the purpose of this proof is to prove 👇
	- it result in that **`dot product` is equals to `matrix multiplication`**

- cross product
	- Just the intuition and inspiration (*added in self-review*)
		- preparation material: [The Geometry of the Dot and Cross Products](zotero://select/library/items/RJWP8A3E)
			- we use $\vec{v}\times\vec{w}=v_{x}w_{x}\hat{i}\times\hat{i}+\dots$, implied that dot product is distributes over **addition**, to *affirm* it, we should first prove that **cross product is linear** (for more details, read [Linear map](https://en.wikipedia.org/wiki/Linear_map))
				- a further reading from the reference book may help to you
				- the proof is to prove $\vec{w}\times(\vec{v}+\vec{u})=\vec{w}\times\vec{v}+\vec{w}\times\vec{u}$, which the cross product is **proportional coordinate** to the sum of each projection component of vectors
		- cross product is to calculate the area of parallelogram
			- there's why $\hat{i}\times\hat{i}$ is equals to zero
		- non-zero cross product will generate a vector that orthogonal to $\vec{v}, \vec{w}$, **by cross product definition itself**
			- try expand $(v_x\hat{i}+v_y\hat{j}+v_z\hat{k})\times(w_x\hat{i}+w_y\hat{j}+w_z\hat{k})$, and you will get $$\left(v_{y} w_{z}-v_{z} w_{y}\right) \hat{\imath}+\left(v_{z} w_{x}-v_{x} w_{z}\right) \hat{\jmath}+\left(v_{x} w_{y}-v_{y} w_{x}\right) \hat{k}$$
		- ANY $\vec{v}\times\vec{w} = 0$ ($\vec{v},\vec{w}\neq0$) demonstrated that $\vec{v}$ and $\vec{w}$ are linear independent ([[3B1B - Linear algebra#^33b084|definition]])
	- usually $\vec{v}\cdot\vec{w}=\vec{p}$ to calculate the $\vec{p}$ that perpendicular to $\vec{v}$ and $\vec{w}$, the sign of $\vec{p}$ follow the right-hand rule (index finger points to $\vec{v}$ and middle finger points to $\vec{w}$)
	- the non-zero cross product indicate the **square area** constitute by $\vec{v}, \vec{w}$, which the length of the vector of product
	- actually, the formula is try to find **a series of $\vec{p}\ (\vec{i},\vec{j},\vec{k})$ that can express a certain determinant $$
	\begin{bmatrix}
	x \\
	y \\
	z
	\end{bmatrix} 
	[p_1\ p_2\ p_3]
	=
	f(\begin{bmatrix}
	x \\
	y \\
	z
	\end{bmatrix})
	=
	\begin{bmatrix}
	v_1 \\
	v_2 \\
	v_3 
	\end{bmatrix}
	\times
	\begin{bmatrix}
	w_1 \\
	w_2 \\
	w_3
	\end{bmatrix}
	=
	\det{(
	\begin{bmatrix}
	\hat{i} & v_1  & w_1 \\
	\hat{j} & v_2  & w_2  \\
	\hat{k} & v_3  & w_3
	\end{bmatrix}
	)}  
	$$
		- consider that $\begin{bmatrix} p_1 & p_2 & p_3 \end{bmatrix}$ stands for **scalar factor**, and $\begin{bmatrix} x \\ y \\ z\end{bmatrix}$ stands for a certain **vector**, *scalar factor* in each row from *top to bottom* correspond to the *vectors* in columns from *left to right*, if we compound 
		- the solution of $\vec{p}$ in one determinant is numerous, a bunch of evenly spaced vectors can mapping the same values to the axis perpendicular to $v$ and $w$
		- the purpose of $det$ is to compute the volume (area) of $v, w$ and $p$. Since it's `linear`, $det$ will evenly spaced for the same spacing
	- you can consider $[x, y, z]$ is do a transformation from space to line, since it takes a input and produce a numerical output and you can use a **linear** multiplication to convert the function $[p_1, p_2, p_3]$, and because of duality that proved above, the transformation can finally represent by a vector $\begin{bmatrix} p_1 \\ p_2 \\ p_3 \end{bmatrix}$
		- it's definitely true by hand-calculation reviewing
		- and with the formula
			- $p_{1}=v_{2} \cdot w_{3}-v_{3} \cdot w_{2}$ 
			- $p_{2}=v_{3} \cdot w_{1}-v_{1} \cdot w_{3}$
			- $p_{3}=v_{1} \cdot w_{2}-v_{2} \cdot w_{1}$
		- it's sufficient to prove that the part of formula generated by $v$ and $w$ is just **a special sign of number**, which $\hat{i},\ \hat{j},\ \hat{k}$ is **certainly a vector**

- basic transformation
	 - it's simple$$A\cdot\ [other's\ vec]=[ours\ vec]$$
	- let's see what $A$ means
		- **A = use ours coordinate to represent other's basis system vectors, it's definitely different from the basis system by other's viewing**,
		- (Video, also the fact) thinking about it takes our misconception of what other means (using the coordinate means by other), then translate it to the vector that ours familiar with
		- *(my thought) thinking about it prepare the translation environment for others' vector, if you do not have the correct mapping from others to ours coordinate system, the manipulation will never make sense.*
- ***The core idea (only available for translation) of computation is applying basis system from left-one so and doing the computing for right-one which likes simulate others manipulation from our basis system***
	- Consider it's **basis system calculation** your viewing with a formula **without** any lower ranking object (the lower object must be appear on right). otherwise, look at it the same way as above 👆
		- Although nothing mentioned, do NOT ignore the first translation is for the basis system $\begin{bmatrix} 1 & 0 \\ 0 & 1\end{bmatrix}$
	- whatever what the inner value is, the result always lies on ours axes but with the meaning that formula itself given
- **And,** it's something like recursion, if you need a left solution, you must calculate the right one first, until you finished all puzzles, it's what function is doing
	- $f(f(f(f(n))))\ \rightarrow \ f(N)$, which $N$ means a object with a lower ranking, and $f(N)$ do translation to gets it's intuitive result in ours basis system
	- so, it's reasonable to understand why $A\cdot[other's\ vec]$ make sense
	- $A^{-1}$ is needless, comprehend what mentioned above is enough, *just restore it by multiple $A^{-1}$ if you no longer need mapping or need to simulate our manipulation from others basic system*
- **Say something may cross the line** #caution #review-needing
	- *DO NOT use transformation BUT translation
	- **I'll try to use** ***translation*** **in the last portion of passage, and see if it works**
- so, how to simulate a manipulation from others perspective?
	- basically: (undo translation) *<-* (apply Main translation from others perspective) *<-* (translate and re-simulate in our views) *<-* (other's vector)`
	- terminologically: $A^{-1}MAX$ (which $X$ a lower rank object)
	- compare with $MAX$, $A^{-1}MAX$ takes both input and output from others perspective
		- if you use $AMA^{-1}X$ rather than $A^{-1}MAX$, things will become worse , since you **use a wrong key to translate basis system form A to B, the result is unexpectable**

- eigenvalue & eigenvector
	- outline
		- eigenvector: the vector which keeps on it's origin span space after transformation (scaling is allowed), the rotation axis of translation
			- it exist and only exist when $A\vec{v}=\lambda\vec{v}\rightarrow (A-\lambda I)\vec{v}=0 \rightarrow \det(A-\lambda I)=0$  is **true** (a set of vectors, equals to $\lambda$ times $\vec{v}$ after translation. the eigenvectors, can be calculated by figured out eigenvalue $\lambda$, **and** you know, any determiner equals to zero guarantee that space will be squished during translation, assuming $x$ and $y$ and deal with the translation equation, you can get the solution of $\vec{v}$ that must be located at zero after space squishing)
		- eigenvalue: the scale factor (with no scaling, the factor is $1$)
	- a transformation may leads to diagonally zeroed if you do transformation with a new basis system that the axes are correspond to eigenvectors.
		- Explanation: eigenvector is spatial symmetry axis (it located in which will not be moved in translation) of translation, if we **oriented** apply eigenvector for translation (with using the basis system which **will not be moved during translation**, rather than $\begin{bmatrix} 1 & 0 \\ 0 & 1\end{bmatrix}$), and after translation, the vectors are guaranteed to be placed at the same orientation as eigenvector, and mapping to the axes after un-translation.
			- * *translation: the main transformation, NOT the transformation by eigenvector*
			- you may lost metadata since it's not doing translation by the perspective from ours
			- but thanks for that mapping, we can easily figure out what happens (the regular) on matrix exponential computation
		- if you find out the result of complications are something **regular** during exponential calculation, it implies that you should have a try 👆
		- exist or not depends on whether your available eigenvector enough to span to whole space
	- *the essence of $AMA^{-1}$ with eigenvector is not been figured out by myself actually*, but luckily I dig out the essence of *transformation*