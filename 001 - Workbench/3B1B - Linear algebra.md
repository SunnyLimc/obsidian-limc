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
		- if a vector that can not be form from any another (`one`, `two` .. `dimension - 1`), the dimension of `span` will increased, named with the terminology: `Linearly independent (线性无关)`  officially written as $\vec{u} \neq b\vec{w}+c\vec{u}$ in three dimension and $\vec{u} \neq a\vec{w}$ in two dimension
		- a terminology to describe the situation that a redundant vector (two vector are in the same line) exist is: `Linearly dependent (线性相关)`
		- the ** `basis` ** is a set of linearly independent vectors that can span the full space.

- linear transformation
	- Principles
		- Lines remain lines
		- Origin remains fixed
	- Grid lines remain parallel and evenly spaced