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
	- add an addition vector $a\vec{v}+b\vec{w}+c\vec{u}$ can get the whole three-dimension span
		- if one of vectors located at which plane of `span` create by the other two ones, the `span` of those 3 vectors can only be a specific plane
		- in other words, the third vec 