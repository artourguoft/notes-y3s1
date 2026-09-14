## <u>12: Vectors and the Geometry of Space</u>
### <u>12.1: Three-Dimensional Coordinate System</u>
Points in the two-dimensional plane $\mathbb{R}\times \mathbb{R}=\mathbb{R}^{2}$ are described by ordered pairs of two real numbers, representing the two axes; points in three-dimensional space $\mathbb{R}\times \mathbb{R}\times \mathbb{R}=\mathbb{R}^{3}$ are described by ordered triples of three real numbers, representing the three axes
- Three-dimensional space then contains the three coordinate planes; the $xy$, $xz$, and $yz$ planes, which are divided into eight **octants** by the axes 

Equations in $\mathbb{R}^{3}$ represent **surfaces** rather than **curves**; the simplest of these surfaces are planes defined by a single coordinate; ex. $z=5$ is the set of all ordered triples whose $z$ coordinate is $5$
- For a constant $c$, $x=c$ is a plane parallel to the $yz$-plane, $y=c$ a plane parallel to the $xz$-plane, and $z=c$ a plane parallel to the $xy$-plane
	- Furthermore, if any of the three variables are **not explicit** in an equation, then the surface described by the equation extends infinitely along that axis; that variable essentially has an implicit $0$ coefficient, allowing it to take any value
		- Ex. $y=3x-2$ is a simple linear function in the $xy$-plane, which in $\mathbb{R}^3$ becomes a plane extending infinitely up and down the $z$-axis, or $z=x^2$ which is a parabola in the $xz$-plane but extends infinitely along the $y$-axis in $\mathbb{R}^3$
- Equations of the form $\sqrt{x^2+y^2}=c$ represent **circles** with radius $c$, it follows that forms like $\sqrt{x^2+y^2}\leq c$ represent filled-in circles; similarly equations of the form $x^2+y^2=c$ represent circles with the radius $\sqrt{c}$
	- In $\mathbb{R}^3$ these equations define surfaces that extend up and down the $z$-axis to form **hollow cylinders** with the radii of the relevant circle
	- If we remove the constant and instead treat these like functions in $\mathbb{R}^{3}$, ex. $f(x,y)=\sqrt{ x^2+y^2 }$ and $f(x,y)=x^2+y^2$, the resulting surfaces are a cup shaped by parabolas and a cone, respectively (this will become clear when cross-sections are introduced)

We can easily derive that the **distance** between two points in $\mathbb{R}^{3}$ is $\sqrt{(x_{2}-x_{1})^{2}+(y_{2}-y_{1})^{2}+(z_{2}-z_{1})^{2}}$
- The equation of a **sphere** centered at $(a,b,c)$ with radius $\sqrt{ r }$ now follows as $(x-a)^{2}+(y-b)^{2}+(z-c)^{2}=r$
- Often we have equations of the form $x^{2}+y^{2}+z^{2}+ax+by+cz+d=0$; these actually represent spheres as well, and we can reveal that by grouping terms and **completing the square**:
	- Ex. $x^{2}+y^{2}+z^{2}+4x+6y+2z+6=0$ goes to $(x^{2}+4x+4)+(y^{2}+6y+9)+(z^{2}+2z+1)=-6+4+9+1$ and finally to $(x+2)^{2}+(y+3)^{2}+(z+1)^{2}=8$

Consider that surfaces representing functions in $\mathbb{R}^3$ can have:
- **Any number** of $x$-**intercepts** and $y$-**intercepts**; this includes the possibilities of zero or infinite intercepts as well
- **Zero or one** $z$-**intercepts**; if the function is defined at the origin then the value $f(0,0)$ is the $z$-intercept, otherwise if $f(0,0)$ is not defined then there is no $z$-intercept 
### <u>12.2: Vectors</u>
**Vectors** indicate quantities that have both **magnitude** and **direction**
- Vectors $\mathbf{a}, \mathbf{b}$ are **equivalent** (ie. $\mathbf{a}=\mathbf{b}$) if they have the same magnitude and direction, even if they are in different positions
- The **zero vector** denoted $\mathbf{0}$ has length $0$ and **no direction**

**Vector Addition:** for vectors $\mathbf{a}, \mathbf{b}$, we have $\mathbf{a}+\mathbf{b}=\mathbf{b}+\mathbf{a}$, which is the vector from the initial point of one vector to the terminal point of the other
- Alternatively, we can view $\mathbf{a}+\mathbf{b}$ as the diagonal on the **parallelogram** created by tracing the two ways to traverse $\mathbf{a}, \mathbf{b}$
- Algebraically, this is simply component-wise addition

**Scalar Multiplication:** for vector $\mathbf{a}$ and scalar $c$, the scalar multiple $c \mathbf{a}$ is the vector whose magnitude is $|c|$ times the length of $\mathbf{a}$, and whose direction is maintained or changed by the sign of $c$ (ie. for $c<0$, the direction is flipped)
- For either $c=0$ or $\mathbf{a}=0$, we have $c \mathbf{a}=\mathbf{0}$
- Algebraically, this is simply component-wise multiplication where $c$ is distributed into each component
- This also helps us define **differences** of vectors, ex. $\mathbf{a}-\mathbf{b}=\mathbf{a}+(-\mathbf{b})$ where $-\mathbf{b}=-1\cdot \mathbf{b}$
	- Geometrically, this difference is also the vector that connects the tip of $\mathbf{b}$ to the tip of $\mathbf{a}$
- This also helps us define **parallel** vectors, which are vectors that are **scalar multiples** of each other (ie. each component is the same scalar multiple of the associated component in the other)

**Magnitude:** 
- For a two-dimensional vector $\mathbf{a}=(a_{1},a_{2})$, we have $\|a\|=\sqrt{a_{1}^{2}+a_{2}^{2}}$
- For a three-dimensional vector $\mathbf{a}=(a_{1},a_{2},a_{3})$, we have $\|a\|=\sqrt{a_{1}^{2}+a_{2}^{2}+a_{3}^{2}}$

Regarding magnitude, we can interpret this as the distance between two points called a **displacement** vector, where for two points $P_{1}=(x_{1},y_{1},z_{1})$ and $P_{2}=(x_{2},y_{2},z_{2})$, the displacement vector is $(x_{1}-x_{2},y_{1}-y_{2},z_{1}-z_{1})$
- Furthermore, in $\mathbb{R}^{2}$ we can use the magnitude and the angle with the $x$-axis $\theta$ to determine the $x$ and $y$ components as $(\|\mathbf{a}\|\cos\theta,\|\mathbf{a\|\sin\theta})$

In $\mathbb{R}^{3}$ it is often useful to consider the **standard basis vectors**; $\mathbf{i}=(1,0,0), \;\;\mathbf{j}=(0,1,0), \;\;\mathbf{k}=(0,0,1)$; these of course have magnitude $1$ and point in the directions of the $x,y,z$ axes respectively
- Then, we can express any vector in $\mathbb{R}^{3}$ by scalar multiplication by the value of each respective **component** matched to the appropriate standard basis vector based on direction, followed by addition of all three (now scaled) standard basis vectors, ie. $\mathbf{v}=a\mathbf{i}+b\mathbf{j}+c \mathbf{k}$
- **Unit vectors** are those which have magnitudes of $1$; the standard basis vectors are a special case of this, but we can determine a unit vector of a vector $\mathbf{a}$ in any direction as $\frac{\mathbf{a}}{\|\mathbf{a}\|}$
### <u>12.3: The Dot Product</u>
**Dot Product:** for vectors $\mathbf{a}=(a_{1},a_{2},\dots,a_{n}),\mathbf{b}=(b_{1},b_{2},\dots,b_{n})$, we define **algebraically** $a\cdot b=a_{1}b_{1}+a_{2}b_{2}+\dots +a_{n}b_{n}$; note that the dot product is a real number, not a vector

Key algebraic properties of dot products for vectors $\mathbf{a},\mathbf{b},\mathbf{c}$ and scalar $c$:
- $\mathbf{a}\cdot \mathbf{a}=\|\mathbf{a}\|^{2}$
- $\mathbf{a}\cdot \mathbf{b}=\mathbf{b}\cdot \mathbf{a}$, ie. commutativity
- $\mathbf{a}\cdot(\mathbf{b}+\mathbf{c})=\mathbf{a}\cdot \mathbf{b}+\mathbf{a}\cdot \mathbf{c}$, ie. distributivity
- $(c \mathbf{a})\cdot \mathbf{b}=\mathbf{a}\cdot(c\mathbf{b})=c(\mathbf{a}\cdot \mathbf{b})$
- $\mathbf{0}\cdot \mathbf{a}=0$

The **geometric interpretation** of the dot product is that for the angle $0\leq\theta\leq \pi$ between vectors $\mathbf{a},\mathbf{b}$, the dot product $\mathbf{a}\cdot \mathbf{b}=\|\mathbf{a\|\|\mathbf{b\|}\cos{\theta}}$
- We can also rearrange to $\cos{\theta}=\frac{\mathbf{a}\cdot {\mathbf{b}}}{\|\mathbf{a\|\|\mathbf{b\|}}}$, depending on what quantities we are given
- It follows that iff two vectors are **orthogonal**, that is $\theta=\frac{\pi}{2}$ or $90^{\circ}$, then $\cos{\theta}=0$ and thus $\mathbf{a}\cdot \mathbf{b}=0$
	- Furthermore, since $\cos{\theta}>0$ for $0\leq\theta<\frac{\pi}{2}$, then $\mathbf{a}\cdot \mathbf{b}>0$ in this interval, and conversely since $\cos{\theta}<0$ for $\frac{\pi}{2}<\theta\leq\pi$, then $\mathbf{a}\cdot \mathbf{b}<0$
- We can think of the dot product as measuring the degree to which two vectors point in the same direction relative to orthogonality, then intuitively:
	- Orthogonal vectors have a dot product of $0$, as we saw algebraically
	- For vectors that are **coincident** to each other and point in the **same** direction, $\theta=0$ and $\cos{0}=1$, so the dot product is just $\|\mathbf{a}\|\|\mathbf{b}\|$
	- For vectors that are **coincident** to each other and point in **opposite** directions, $\theta=\pi$ and $\cos{\pi}=-1$, so the dot product is just $-\|\mathbf{a}\|\|\mathbf{b}\|$


To determine the **standard form of a plane**, we only need a point $P_{0}=(x_{0},y_{0},z_{0})$ on the plane and a normal vector $\mathbf{n}=a\mathbf{i}+b\mathbf{j}+c\mathbf{k}$ to the plane
- Then all displacement vectors from $P_{0}$ that are perpendicular to the normal vector define the plane
- Take an arbitrary point $P_{1}=(x,y,z)$ in the plane, then the displacement vector $P_{0}P_{1}=(x-x_{0},y-y_{0},z-z_{0})$ must be **perpendicular to the normal** vector, so $\mathbf{n}\cdot P_{0}P_{1}=0$ by properties of the dot product, from which we get the equation $a(x-x_{0})+b(y-y_{0})+c(z-z_{0})=0$
	- We can simplify further; let $d=ax_{0}+by_{0}+cz_{0}$, then $ax+by+cz=d$; note the components of the normal vector are clear in both forms!


**Scalar Projection:** also called the **component** of $\mathbf{b}$ along $\mathbf{a}$, is the **magnitude** of the 'shadow' of vector $\mathbf{b}$ in the direction of vector $\mathbf{a}$:
$$
\text{comp}_{\mathbf{a}}\mathbf{b}=\frac{\mathbf{a}\cdot \mathbf{b}}{\|\mathbf{a}\|}={\|\mathbf{b}\|}\cos\theta
$$
**Vector Projection:** also called the **projection** of $\mathbf{b}$ along $\mathbf{a}$, is the **vector** which constitutes the superimposed 'shadow' of a vector $\mathbf{b}$ in the direction of vector $\mathbf{a}$:
$$
\text{proj}_{\mathbf{a}}\mathbf{b}=\text{comp}_{\mathbf{a}}\mathbf{b} \;\frac{\mathbf{a}}{\|\mathbf{a}\|}=\left(\frac{\mathbf{a}\cdot \mathbf{b}}{\|\mathbf{a}\|}\right) \frac{\mathbf{a}}{\|\mathbf{a}\|}
$$
Notice that the vector projection is the component-scaled unit vector in the direction of $\mathbf{a}$

**Vector Rejection:** also called the **rejection** of $\mathbf{b}$ along $\mathbf{a}$, is the displacement **vector** from the tip of the projection to the tip of the original vector: 
$$
\text{rej}_{\mathbf{a}}\mathbf{b}=\mathbf{b}-\text{proj}_{\mathbf{a}}\mathbf{b}
$$
Consider that the **rejection is parallel to the projection** by definition
### <u>[13.4]: The Cross Product</u>
Recall that for any two vectors $\mathbf{a},\mathbf{b}\in \mathbb{R}^{3}$, if we trace the two ways of adding these vectors we get a parallelogram; the formula for the area of a parallelogram is $\text{base} \times \text{height}$, which in vector terms is equivalent to $\|\mathbf{a}\|\|\mathbf{b}\|\sin\theta$ assuming $\mathbf{a}$ as the base
- However, if we are given only the components of these two vectors, calculating the magnitudes and the angle (through the dot product) can be a long process; instead, we use the cross product

**Cross Product:** for vectors $\mathbf{a},\mathbf{b}\in \mathbb{R}^{3}$ with angle $0\leq\theta<\pi$ between them, the cross product $\mathbf{a}\times \mathbf{b}$ is a vector **perpendicular to both** $\mathbf{a},\mathbf{b}$, whose **magnitude** $\|\mathbf{a}\times \mathbf{b\|}$ is **equal to the area of the parallelogram** defined by tracing addition of $\mathbf{a},\mathbf{b}$
- Given this definition, the cross product must be a normal vector $\mathbf{n}$ to the plane defined by $\mathbf{a},\mathbf{b}$

**Geometrically**, for non-parallel vectors $\mathbf{a},\mathbf{b}\in \mathbb{R}^{3}$ with angle $0\leq\theta<\pi$ between them:
$$
\mathbf{a}\times \mathbf{b} = (\|\mathbf{a}\|\|\mathbf{b}\|\sin\theta)\mathbf{n}
$$
This is the area of the parallelogram (and the magnitude of the cross product) multiplied as a **scalar** into the normal vector for the given plane
- This also suggests that the cross product is **rotation invariant**; if we connect $\mathbf{a},\mathbf{b}$ at their roots and rotate the resulting object around, only the direction of the cross product (defined by $\mathbf{n}$) will change, not the magnitude

**Algebraically**:
$$
\mathbf{a}\times \mathbf{b} = (a_{2}b_{3}-a_{3}b_{2})\mathbf{i} + (a_{3}b_{1}-a_{1}b_{3})\mathbf{j} + (a_{1}b_{2}-a_{2}b_{1})\mathbf{k}
$$
An easier way to remember this formula is to write the cross product as a $3\times 3$ **determinant**, which is of course defined by $2\times 2$ determinants:
$$
\begin{vmatrix}
\mathbf{i} & \mathbf{j} &\mathbf{k} \\
a_{1} & a_{2} & a_{3} \\
b_{1} & b_{2} & b_{3}
\end{vmatrix}
=\mathbf{i}
\begin{vmatrix}
a_{2} & a_{3} \\
b_{2} & b_{3}
\end{vmatrix}
- \mathbf{j}
\begin{vmatrix}
a_{1} & a_{3} \\
b_{1} & b_{3}
\end{vmatrix}
+ \mathbf{k}
\begin{vmatrix}
a_{1} & a_{2} \\
b_{1} & b_{2}
\end{vmatrix}
= (a_{2}b_{3}-a_{3}b_{2})\mathbf{i} - (a_{1}b_{3}-a_{3}b_{1})\mathbf{j} + (a_{1}b_{2}-a_{2}b_{1})\mathbf{k}
$$
Note the second term is subtracted and the order of subtraction inside the brackets of that term is fixed to be the same as the rest of the terms (by the definition of determinants); this is ultimately **equivalent** to the algebraic definition of cross products above (and arguably easier to remember)
- Also, note the algebraic method spares us from having to determine a normal vector first


Recall, the **standard form of a plane** can be found from a single point on the plane and a normal vector; with the cross product, we only need $3$ points on the plane with which we can determine two displacement vectors, and use to the cross product to determine a normal vector to the plane 


Key algebraic properties of cross products:
- $(\mathbf{a}\times \mathbf{b})\cdot \mathbf{a}=(\mathbf{a}\times \mathbf{b})\cdot \mathbf{b}=0$ (the **dot product** of the cross product with **either vector** is $0$, because it is perpendicular to them)
- $\mathbf{a}\times \mathbf{b}=-(\mathbf{b}\times \mathbf{a})$ (the cross products point in opposite directions)
- $c(\mathbf{a}\times \mathbf{b})=(c \mathbf{a})\times \mathbf{b}=\mathbf{a}\times (c\mathbf{b})$
- $\mathbf{a}\times(\mathbf{b}+\mathbf{c})=(\mathbf{a}\times \mathbf{b})+(\mathbf{a}\times \mathbf{c})$
## <u>14: Partial Derivatives</u>
### <u>14.1: Functions of Several Variables</u>
A **function** $f$ **of two variables** is a rule that assigns to each ordered pair of real numbers $(x,y)$ in a set $D$, a unique real number denoted $f(x,y)$
- Here $x,y$ are **independent variables**, and $f(x,y)$ is the **dependent variable** (often labelled $z$, where $z=f(x,y)$)
- Visually, we intuited single variable real-valued functions as a map from the real number line to the real number line (one being the $x$-axis and the other the $y$-axis); real-valued functions of two variables are maps from the $xy$-**plane** to the real number line (the $z$-axis)
	- The domain is some subset $D\subseteq \mathbb{R}^{2}$, and the codomain $\mathbb{R}$
	- Graphs of two-variable functions are then sets of all points $(x,y,z)\in \mathbb{R}^{3}$ such that $(x,y) \in D$ and $z=f(x,y)$

Functions of two variables **can exclude one or both of the variables**; ex. $f(x,y)=x^{2}$ is a parabola opening upward on the $z$-axis and extending infinitely in both directions along the $y$-axis
- In $\mathbb{R}^2$ there was only one dependent variable, thus if it was not explicitly included in the formula the result was simply a straight line with constant $c=y$; in $\mathbb{R}^3$ this happens only when both dependent variables are excluded ex. $z=c$ as described in [[#<u>12.1 Three-Dimensional Coordinate System</u>|Section 12.1]]
- Alternatively we can have equations (not functions) ex. $y=x^{2}$ which is a parabola extending infinitely in both directions along the $z$-axis

The **contours** or **level curves** of a function of two variables are curves in the $xy$-plane with equations $f(x,y)=c$ for a constant $z=c$ chosen from the range of $f$; these are sets of all points at which $f(x,y)$ is $c$ height away from the $xy$-plane
- Level curves are essentially **horizontal slices** of the graph of $f(x,y)$, superimposed on the $xy$-plane
	- To **determine a level curve algebraically** we substitute $z=c$ which gives us the equation of the level curve in the $xy$-plane (which itself is not always a function) and thus the set of $(x,y)$ such that $f(x,y)=c$
- A collection of level curves for various values of $z$ is a **contour map**
- Contour maps are most useful when we take **equally spaced values of** $z$; it follows that for such contour maps, level curves that are **closer together indicate a more rapid change** in the underlying function
- Level curves **cannot intersect** as it would mean that the underlying function is invalid (multiple values mapped per domain element)

**Cross-sections** are similar to level curves, but they are defined by **vertical** (or otherwise non-horizontal) planes, compared to level curves which are defined by strictly horizontal planes
- Commonly useful cross-sections are those defined by planes where $x$ or $y$ are held constant; then we are left with $z=f(x,c)$ or $z=f(c,y)$ which are vertical slices where the curve of $z$ as function of only one of $x$ or $y$

Functions of **three or more variables** are defined similarly, but of course difficult to visualize; nevertheless, we can gain some insight by examining the **level surfaces** of three-variable functions, which are surfaces in $\mathbb{R}^{3}$ with equations $f(x,y,z)=c$ where $c$ is a constant
- Level surfaces consist of the ordered triples for which $f(x,y,z)$ will have the same value of $f$ 
- These contour diagrams are **collections of level surfaces**, and we can **algebraically determine an equation** in $\mathbb{R}^{3}$ for a surface by isolating $z$ for each given $c$ and treating it as function of two variables
	- Note, this is **not always possible**
		- The resulting equation may be an invalid function; when $z$ does not appear in the original function at all, or when $z$ is composed with even powers or even radicals (then the resulting equation will have a $\pm$, but we can make this valid by choosing one side)
		- If $z$ appears both inside linear or polynomials terms and transcendental terms (ex. $e^z$ or trigonometric functions) in the original function then there is no algebraic way to isolate $z$, even though a unique $z$ may still exist for every $(x, y)$
	- **Conversely**, we can also determine a three-variable function $g$ for which a two-variable function $f$ is the level surface for $g(x,y,z)=0$ by setting $g(x,y,z)=f(x,y)-z=0$; note this is a function that has this level curve, but this $g$ is **not unique** by any means!
### <u>[12.4]: Linear Functions</u>
**Linear functions** of two variables take the form $z=ax+by+c$ (often given as as $ax+by-z=-c$) and represent **planes** in $\mathbb{R}^{3}$
- **Any** other composition will make the functions nonlinear (ex. $z=xy$, or $z=x^2+y$, etc.)
- The defining feature of a linear function of two variables is that the **slopes** $\frac{\Delta z}{\Delta x}$ at a fixed $y$ and $\frac{\Delta z}{\Delta y}$ at a fixed $x$ are **constant** (in cross-section terms; the cross-sections of a plane are lines)
	- These slopes are also the same **regardless of the fixed value**; in fact, the slope of a plane along any **one direction** is always the same constant regardless of the initial point
	- These properties of linear functions are also visible in **tabular form**, where the **rows and columns each have constant linear increments** throughout (ie. same slope between all rows, and another between all columns)
	- These properties of linear functions are also visible in contour maps, where all level curves are **parallel**, **equally-spaced** straight lines each of which represent the linear combinations of $(x,y)$ for that given $z$

For a linear function, if we know the value of the function at some given point $(x_0,y_0,z_0)$ and we know the slope $m$ in the $x$ direction and $n$ in the $y$ direction, then we can determine the function as the **point-slope form of a plane**: 
$$
\begin{align}
f(x,y)&=z=m(x-x_0)+n(y-y_0)+z_0 \\
f(x,y)&=z=mx+ny+c, \quad \text{where the $z$-intercept is $c=z_0-mx_0-ny_0=f(0,0)$}
\end{align}
$$
Note, there are also **directional slopes** as we move along any other line on the $xy$ plane; more on that topic later, for now we consider only the slopes above. Also note this can easily be turned into the **standard form of a plane**:
$$
f(x,y)=z=m(x-x_0)+n(y-y_0)+z_0 \iff mx+ny-z=mx_{0}+ny_{0}-z_{0}
$$

Overall, we are equipped for several scenarios with **determining equations of planes**; a plane can be determined by just:
1. A single point and both slopes:
	- A given $(x_{0},y_{0},z_{0})$ or $f(x_{0},y_{0})$ and given slopes allow us to simply express the plane as a function $z=m(x-x_0)+n(y-y_0)+z_0$
	- Note, we can easily rearrange this function format into the standard form like $-mx-ny+z=-mx_{0}-ny_{0}+z_{0}$
2. A single point and a normal vector:
	- A given $(x_{0},y_{0},z_{0})$ or $f(x_{0},y_{0})$ and a normal vector $\mathbf{n}=a\mathbf{i}+b\mathbf{j}+c\mathbf{k}$ allow us to express the plane in standard form relative to an arbitrary displacement vector $(x,y,z)$ where $a(x-x_{0})+b(y-y_{0})+c(z-z_{0})=0$, or $ax+by+cz=d=ax_{0}+by_{0}+cz_{0}$
3. A set of $3$ points, provided all $3$ points **do not lie on the same line**:
	- If we are given points that pairwise lie parallel to the $x$ and $y$ axes, then we can simply use the difference in $z$ between those points to calculate $m,n$, and use those with any of the given points as $(x_{0},y_{0},z_{0})$ to determine our function as in method *1*
	- If we are not given points that pairwise lie along axes, we use the displacement vectors to cross product to normal vector method instead, and then refer to method *2*

Lines in $\mathbb{R}^3$ are fully determined by a point and a parameterized direction vector; planes are not fully determined by a point and a contained vector because we also need their orientation
- The orientation of a plane is determined by a normal vector to the plane; normal vectors to a plane are parallel to **all** vectors contained in the plane
	- Two planes are parallel if their normal vectors are parallel (ie. the normal vectors are scalar multiples of each other)
	- If two planes are not parallel, then they necessarily **intersect in a straight line** (the set of points which satisfies both equations is the line), and the angle between the two planes is defined as the acute angle between their normal vectors
### <u>14.2: Limits and Continuity</u>
For a function $f$ whose domain includes points arbitrarily close to but not necessarily equal to $(a,b)$, we say that the **limit** of $f(x,y)$ as $(x,y)$ approaches $(a,b)$ is $L$ as:
$$
\lim_{ (x,y) \to (a,b) } f(x,y) = L
$$
Recall, for single variable functions the **definition of the limit** was that the difference between $f(x)$ and $L$ can be made arbitrarily small by bounding the distance $0<|x-a|<\delta$
- Now our domain is the $xy$-plane rather than the $x$-axis, thus the definition changes to where the $\delta$ is a **radius** around the point $(a,b)$ on the $xy$-plane, ie. $0<\sqrt{(x-a)^{2}+(y-b)^{2}}<\delta$

For single variable functions, determining that a limit **does not exist** involved approaching the value $a$ from both directions on the $x$-axis, from where if we had $\lim_{x\to a^{-}} f(x) \neq \lim_{x\to a^{+}} f(x)$ then the limit did not exist
- For functions of two variables, we can approach $(a,b)$ from an infinite number of directions in the $xy$-plane, and in any manner (ie. not just straight lines but functions in the $xy$-plane)
- However, our definition of the limit above only specified that we must be within a certain radius $\delta$, so if a limit exists then it must approach the same value regardless of where specifically within the circle of radius $\delta$ the direction of approach lies
- All **limit laws** apply the same as they did for single variable functions

We can show that a **limit does not exist** by finding **at least two paths** along which the function takes on different values as we approach $(a,b)$; the simplest paths to check are the $x$-axis and $y$-axis since one value will remain constant throughout in each path
- Other simple paths to check are $y=x$ where we can just substitute $y$s for $x$s in the function, or more generally $y=mx$ for any given $m$ which allows us to substitute $mx$s and get a parameterized expression for any given straight line through the origin in the $xy$-plane
- Note, we can pick **any curve as a path**, for ex. $y=x^2$ etc., as long as it **approaches the point** in the limit, ex. take $y=kx^2$ for any $k\neq 0$:
$$
\lim_{ (x,y) \to (0,0) } \frac{x^2}{x^2+y}=\lim_{ x \to 0 } \frac{x^2}{x^2+kx^2}=\lim_{ x \to 0 } \frac{x^2}{(k+1)x^2}=\lim_{ x \to 0 } \frac{1}{k+1} \quad (\text{infinite values for infinite }k\text{s})
$$

Conversely, to prove the **existence and value** of a limit we can:
- Use knowledge of continuity of the function at $(a,b)$ to simply evaluate it and immediately deduce the existence and value of the limit
- Use the the Squeeze Theorem

**Continuity** is defined once again by the property:$$\lim_{(x,y) \to (a,b)} f(x,y) = f(a,b)$$We say that $f$ is **continuous on its domain** if it continuous at each point in its domain
- Limits of known continuous functions, like polynomials or quotients of them, can again be evaluated by simply substituting in the point $(a,b)$
- It follows that we can determine **discontinuity** at a point $(a,b)$ either by showing that the **limit does not exist** as before, or by showing $\lim_{(x,y) \to (a,b)} f(x,y) \neq f(a,b)$

The **Squeeze Theorem** holds for multiple variable functions; this is often useful when $f$ is a quotient of polynomials; then we can bound it by terms in the numerator, and if the bounds converge to the same limit then so does $f$
- Ex.: for $f(x,y)=\frac{5xy^4}{x^2+y^2}$, determine existence and value of $\lim_{ (x,y) \to (0,0) }{f(x,y)}$
	- Consider that around $(0,0)$ where $-1\leq y\leq 1$ we have $y^4\leq y^2$, then $\frac{y^4}{x^2+y^2}\leq \frac{y^2}{x^2+y^2}\leq 1$
	- Thus around $(0,0)$ we have $-5x\leq f(x)\leq 5x$; both bounds are continuous functions at $(0,0)$ and have limit $0$
	- So by the Squeeze Theorem we must have that $\lim_{ (x,y) \to (0,0) }{f(x,y)}=0$
- Ex.: for $f(x,y)=(x^2+y^2)\cdot \left( \sin\left( \frac{1}{x^2+y^2} \right) \right)$, determine existence and value of $\lim_{ (x,y) \to (0,0) }{f(x,y)}$
	- Consider that $\forall a\in \mathbb{R},-1\leq\sin a\leq 1$, then we have $\forall(x,y)\in \mathbb{R}^2,-(x^2+y^2)\leq f(x,y)\leq (x^2+y^2)$
	- Both bounds $g(x,y)=x^2+y^2$ and $h(x,y)=-(x^2+y^2)$ are continuous at $(0,0)$ and $\lim_{ (x,y) \to (0,0) }{g(x,y)}=\lim_{ (x,y) \to (0,0) }{h(x,y)}=0^2+0^2=0$
	- Thus, by the Squeeze Theorem we must have that $\lim_{ (x,y) \to (0,0) }{f(x,y)}=0$
### <u>14.3: The Partial Derivative</u>
For a function of two variables, we define partial derivatives with regards to the $x$ and $y$ axes, where we hold the $y$ and $x$ values **constant** respectively:
- **Partial derivative with respect to** $x$: $f_{x}(x,y)=\frac{\partial}{\partial x}f(x,y)=\frac{\partial}{\partial x}z=\lim_{ h \to 0 }\frac{f(x+h,y)-f(x,y)}{h}$
- **Partial derivative with respect to** $y$: $f_{y}(x,y)=\frac{\partial}{\partial y}f(x,y)=\frac{\partial}{\partial y}z=\lim_{ h \to 0 }\frac{f(x,y+h)-f(x,y)}{h}$
To determine these derivative functions, we hold the relevant variable constant and simply differentiate the resulting single variable function as usual. This also suggests that various differentiation rules, the **chain rule**, etc. all hold.

To calculate the partial derivatives at a point $(a,b)$, we substitute these values into our determined partial derivative functions. Considering the geometric interpretation of these partial derivatives, we could also estimate them if we are provided a **contour diagram** of the function.

The **units** of a partial derivative are the units of the scalar codomain of the function divided by the units of the independent variable which we are differentiating with respect to. Recall, the derivative is the rate of change for an infinitesimal interval, but that rate of change is still measured **per unit** of the independent variable (recall, slope of the tangent line at the point of differentiation, etc.)

Since these partial derivatives **are themselves functions of two variables**, we can define **higher order partial derivatives** from them as well, with the orderings of what we are differentiating with respect to increasing each time. For example, the second order partial derivatives:
- $(f_{x})_{x}=f_{xx}=\frac{\partial}{\partial x}(\frac{\partial}{\partial x}f)=\frac{\partial^{2}}{\partial x^{2}}f=\frac{\partial^{2}}{\partial x^{2}}z=\lim_{ h \to 0 }\frac{f_{x}(x+h,y)-f_{x}(x,y)}{h}$
- $(f_{y})_{y}=f_{yy}=\frac{\partial}{\partial y}(\frac{\partial}{\partial y}f)=\frac{\partial^{2}}{\partial y^{2}}f=\frac{\partial^{2}}{\partial y^{2}}z=\lim_{ h \to 0 }\frac{f_{y}(x,y+h)-f_{y}(x,y)}{h}$
- $(f_{x})_{y}=f_{xy}=\frac{\partial}{\partial y}(\frac{\partial}{\partial x}f)=\frac{\partial^{2}}{\partial y \partial x}f=\frac{\partial^{2}}{\partial y \partial x}z=\lim_{ h \to 0 }\frac{f_{x}(x,y+h)-f_{x}(x,y)}{h}$
- $(f_{y})_{x}=f_{yx}=\frac{\partial}{\partial x}(\frac{\partial}{\partial y}f)=\frac{\partial^{2}}{\partial x\partial y}f=\frac{\partial^{2}}{\partial x \partial y}z=\lim_{ h \to 0 }\frac{f_{y}(x+h,y)-f_{y}(x,y)}{h}$

**Clairaut's Theorem:** suppose $f$ is defined on a disk that contains the point $(a,b)$, and **all four second order partial derivatives exist and are continuous** on this disk (meaning $f$ is **smooth**), then $f_{xy}(a,b)=f_{yx}(a,b)$
	- This definition of smoothness gives us the class of functions called $C^2$; note there are other non equivalent definitions!

Partial derivatives can also be defined for functions of three or more variables, but they are still defined as the change of the dependent variable with respect to a **single independent variable** with all other variables held constant.


If the first and second order partial derivatives of $f$ around $(a,b)$ **exist and are continuous**, then the **Taylor Polynomials** of degree one and two for $(x,y)$ around $(a,b)$ respectively are:
- $f(x,y)\approx L(x,y)=f(a,b)+f_{x}(a,b)(x-a)+f_{y}(a,b)(y-b)$
- $f(x,y)\approx Q(x,y)=f(a,b)+f_{x}(a,b)(x-a)+f_{y}(a,b)(y-b)+\frac{f_{xx}(a,b)}{2}(x-a)^2+f_{xy}(a,b)(x-a)(y-b)+\frac{f_{yy}(a,b)}{2}(y-b)^2$
Recall, the property we need is that taking the partial derivatives of the Taylor Polynomial gives us the exact partial derivatives of the function.
### <u>14.4: Tangent Planes and  Linear Approximations</u>
Recall **local linearity**; as we look at increasingly smaller intervals of a **differentiable** single variable function, it begins to resemble a linear function; specifically the function of the line tangent to the centre of the interval

Similarly for functions of two variables, as we zoom on to a point on the surface where the underlying function is **differentiable**, the surface begins to resemble the plane tangent to that point, and said plane is of course described by a linear function of two variables
- Specifically, the $x$ and $y$ direction slopes of the tangent plane are described by the respective partial derivatives at the given point
- This can also be observed as we zoom in to a contour diagram of a function; it begins to resemble the **level curves of a linear function**, ie. equally spaced, parallel lines

Consider a surface $S$ defined by a function $z=f(x,y)$ which has **continuous partial derivatives**. Then for a point $P(x_{0},y_{0},z_{0})$ on $S$, consider the cross sections defined by $y=y_{0}$ and $x=x_{0}$. The derivatives of the two curves defined by these cross sections are the partial derivatives $f_{x}(x,y_{0})$ and $f_{y}(x_{0},y)$. Then, the **tangent plane** to $P$ is the plane that **contains the tangent lines of these two curves specifically** at $y_{0}$ and $x_{0}$.
- In fact, the tangent plane contains **all possible tangent lines** of all curves that **lie on** $S$ **and pass through** $P$
- Recall, a plane can be defined from a normal vector $\mathbf{n}=a\mathbf{i}+b\mathbf{j}+c\mathbf{k}$, one point $P(x_{0},y_{0},z_{0})$ and one variable point to get a displacement vector, then: $a(x-x_{0})+b(y-y_{0})+c(z-z_{0})=0$ from which $\frac{a}{c}(x-x_{0})+\frac{b}{c}(y-y_{0})+z_{0}=z$
	- If we want this equation to represent our tangent plane, then its intersection with the plane $y=y_{0}$ must be the line tangent to the curve of the cross-section in this plane (recall, the intersection of two non-parallel planes is a straight line)
	- With some linear algebra substitution ($x_{0},y_{0},z_{0}$ are just number constants, not variables!) we end up with the equation for this intersection line of $\frac{a}{c}(x-x_{0})+z_{0}=z$, which we recognize as a point-slope equation of a line in $\mathbb{R}^2$ with slope $\frac{a}{c}$; but we know the slope is $f_{x}(x_{0},y_{0})$, so we perform the same process for the intersection with the plane $x=x_{0}$, and find that $\frac{a}{c}=f_{x}(x_{0},y_{0})$ and $\frac{b}{c}=f_{y}(x_{0},y_{0})$, thus:
$$f_{x}(x_{0},y_{0})(x-x_{0})+f_{y}(x_{0},y_{0})(y-y_{0})+z_{0}=z$$
	- Note, $x_{0},y_{0}$ are just constants given by our chosen point, as is $z_{0}$ which is just $f(x_{0},y_{0})$, as are $f_{x}(x_{0},y_{0}),f_{y}(x_{0},y_{0})$ since these are evaluated at the specific point; we can make it clearer that this equation is a **local linearization** of $f(x,y)$ by rewriting for $P(a,b,f(a,b))$ as:
$$f_{x}(a,b)(x-a)+f_{y}(a,b)(y-b)+f(a,b)=z$$
	- And finally by local linearity, we claim that the function is approximated by the tangent plane; $f(x,y)\approx f_{x}(a,b)(x-a)+f_{y}(a,b)(y-b)+f(a,b)$, which is the **linear approximation** of $f$
	- Local linearizations of functions of three or more variables follow the same pattern  


Recall for single variable functions, the tangent line at point $x=a$ is $y=f(a)+f'(a)(x-a)$ and the **differentials** are:
- $dx$, the independent variable where $dx=\Delta x$
- $dy$, the dependent variable where $dy=f'(x)dx$, so $dy$ is the **change in** $y$ in the tangent line equation above for each given change in $x$, compared to $\Delta y$ which is the change in the value of the function ($dy$ approximates this for small $\Delta x$)
For differentiable functions of two variables:
- $dx,dy$ are independent variables where $dx=\Delta x,dy=\Delta y$
- $dz$ (or $df$) is the **total differential** where $dz=f_{x}(x,y)dx+f_{y}(x,y)dy=\frac{\partial z}{\partial x}dx+\frac{\partial z}{\partial x}dy$, so $dz$ is the **change in** $z$ in the **tangent plane**, compared to $\Delta z$ which is the change in the value of the function ($dz$ approximates this for small $\Delta x, \Delta y$)
	- The differential is essentially the local linearization without taking into account $f(a,b)$ and the difference from point $(a,b)$; ie. $dx$ and $dy$ instead of $(x-a)$ and $(y-b)$
For differentiable functions of three or more variables, this pattern continues as expected.
### <u>14.5: The Chain Rule</u>
With single variable functions that are compositions of several functions, we often ended up having to do long complicated sequences of applications of the product and quotient rules, followed by the chain rule, etc. Multivariable calculus introduces a more general chain rule from which those rules follow.

Recall the single variable Chain Rule for $f(g(x))$: 
$$
\frac{df}{dx}=\frac{df}{dg}\frac{dg}{dx}
$$

**The Multivariable Chain Rule:** suppose $f$ is a differentiable function of the $n$ variables $x_{1},x_{2},\dots,x_{n}$, and each $x_{j}$ is itself a differentiable function of some combination of the $m$ variables $t_{1},t_{2},\dots,t_{m}$, then $f$ is a function of $t_{1},t_{2},\dots,t_{m}$ and for each $i=1,2,\dots,m$:
$$
\frac{\partial f}{\partial t_{i}}=\frac{\partial f}{\partial x_{1}}\frac{\partial x_{1}}{\partial_{t_{i}}}+\frac{\partial f}{\partial x_{2}}\frac{\partial x_{2}}{\partial_{t_{i}}}+\cdots+\frac{\partial f}{\partial x_{n}}\frac{\partial x_{n}}{\partial_{t_{i}}}
$$
Note this is a **partial derivative**; $f$ is a function of the $m$ variables $t_{1},t_{2},\dots,t_{m}$ but we can only calculate the derivative with respect to one variable at a time
- Each $x_{j}$ is called an **intermediate variable** and each $t_{i}$ is called an **independent variable**
- The simplest scenario is an $f(x,y)$ where $x=g(t)$ and $y=h(t)$, then we have a **single independent variable** $t$ and we could do single variable differentiation, but the multivariable version $\frac{df}{dt}=\frac{\partial f}{\partial x}\frac{dx}{dt}+\frac{\partial f}{\partial y}\frac{dy}{dt}$ reveals where the single variable differentiation rules come from
	- Notice that this is a regular derivative and contains some partials and some regular derivatives inside, since this a just single variable function which we are breaking down into multiple functions
- Another common scenario is $f(x,y)$ where $x=g(t,s)$ and $y=h(t,s)$, then we can determine two partials:
	- $\frac{\partial f}{\partial t}=\frac{\partial f}{\partial x}\frac{\partial x}{\partial t}+\frac{\partial f}{\partial y}\frac{\partial y}{\partial t}$
	- $\frac{\partial f}{\partial s}=\frac{\partial f}{\partial x}\frac{\partial x}{\partial s}+\frac{\partial f}{\partial y}\frac{\partial y}{\partial s}$
- These scenarios can get fairly involved if the intermediate variables are functions of various combinations of the independent variables (in which case some of their partials would be $0$ as they are not dependent on all the independent variables)
	- It is helpful to think of these partial derivatives as a tree graph with each edge being a partial derivative of the upper depth with respect to the lower
		- The top node is the dependent variable to which each intermediate variable is connected, and the relevant independent variables are then connected to each intermediate
		- Then, if we know the partial we want to determine, we go down the tree through edge to each intermediate and from there to the relevant independent and product those partials, and then sum all these products from each intermediate
- The given values vary in questions regarding the Chain Rule
	- Sometimes the **values of the partials of the intermediate variables** are given; those can just be substituted into the Chain Rule expression directly
	- Most of the time the **expressions of the intermediate variables with regards to the independent variables** are given; then we **re-express the intermediate partials in terms of the relevant independent variable**
### <u>14.6: Directional Derivatives and the Gradient Vector</u>
Partial derivatives with respect to $x,y$ gave us the rates of change in the positive directions of the axes, that is in the directions of the unit vectors $\mathbf{i}$ and $\mathbf{j}$. As it were, partial derivatives were just special cases of directional derivatives. We can determine the rate of change in any direction marked by an arbitrary **unit** vector $\mathbf{u}$. In $\mathbb{R}^3$, this is the rate of change of the curve that is the intersection of the vertical plane defined by $\mathbf{u}$ and the surface of the function.

**Directional Derivative:** for a function $f$, the directional derivative in the direction of a **unit vector** $\mathbf{u}=(u_{x},u_{y})$
$$
f_{\mathbf{u}}(x,y)=\lim_{ h \to 0 }\frac{f(x+hu_{x},y+hu_{y})-f(x,y)}{h}
$$
The direction vector must be a unit vector as a consequence of the definition of derivative as the **per unit** instantaneous rate of change
- With single variable functions, $h$ itself was the distance per unit of which we were measuring the slope of the tangent line
- Now $h$ becomes a scalar on the direction vector, and since per unit instantaneous rate of change implies $h=1$, to get $h\cdot||\mathbf{u}||=1\cdot||\mathbf{u}||=1$ we require $||\mathbf{u}||=1$; ie. $\mathbf{u}$ has to be a unit vector

Recall from local linearity that the differential $f_{x}(x,y)dx+f_{y}(x,y)dy$ locally approximates $\Delta f$ by $df$ given that $f$ is **differentiable** at $(x,y)$ (meaning both partials exist and are continuous; will define this in Chapter 14.8), thus as $h\to 0$:
$$
\begin{align}
f_{\mathbf{u}}(x,y)=\frac{\Delta f}{h}&=\frac{f_{x}(x,y)((x+hu_{x})-x)+f_{y}(x,y)((y+hu_{y})-y)}{h} \\
&=f_{x}(x,y)u_{x}+f_{y}(x,y)u_{y}
\end{align}
$$
If $f$ is **not differentiable** at a point $(x,y)$ (meaning one or both partials do not exist), **we cannot conclude anything about the existence or values of  directional derivatives** at that $(x,y)$. These derivatives would have to be determined from the full definition, without the shortcut identity above.

Then we can also conclude:
- $f_{\mathbf{i}}(a,b)=f_{x}(a,b)$
- $f_{\mathbf{j}}(a,b)=f_{y}(a,b)$
- If $\mathbf{v}$ is not a unit vector, then $f_{\mathbf{v}}(a,b)=f_\frac{\mathbf{v}}{\|\mathbf{v}\|}(a,b)$; it is often more convenient to express this as $(\nabla f(a,b)\cdot \mathbf{v})\frac{1}{\|\mathbf{v}\|}$


Note that the definition of the directional derivative above can be expressed as a dot product of two vectors in $\mathbb{R}^2$:
$$
\begin{align}
f_{\mathbf{u}}(x,y)&=f_{x}(x,y)u_{x}+f_{y}(x,y)u_{y} \\
&=(f_{x}(x,y),f_{y}(x,y))\cdot (u_{x},u_{y}) \\
&=(f_{x}(x,y),f_{y}(x,y))\cdot \mathbf{u} \\
&=\nabla f(x,y)\cdot \mathbf{u}
\end{align}
$$
The former of the vectors is called the **gradient** vector of $f$, notated as $\text{grad}f$ or more commonly as $\nabla f$, can be expressed as:
$$
\nabla f=(f_{x}(x,y),f_{y}(x,y))=\frac{\partial f}{\partial x}\mathbf{i}+\frac{\partial f}{\partial y}\mathbf{j}
$$
From this definition, we deduce that the gradient must point in the exact direction of the **greatest directional derivative** at a point:
- The directional derivative is measured per unit; $||\mathbf{u}||=1=\sqrt{u_{x}^2+u_{y}^2 }\implies u_{y}=\sqrt{1-u_{x}^2}\implies f_{\mathbf{u}}(x,y)=f_{x}(x,y)u_{x}+f_{y}(x,y)(\sqrt{1-u_{x}^2})$ which is a single variable function of $u_{x}$ (the partial derivatives are constants at a given point)
- Then to get the critical points of $f_{\mathbf{u}}(x,y)$ with respect to $u_{x}$ we set $\frac{d}{d u_{x}}f_{\mathbf{u}}(x,y)=0=f_{x}(x,y)-f_{y}(x,y)\left(\frac{-u_{x}}{\sqrt{1-u_{x}^2}}\right)$ from which we simplify to $\frac{f_{x}(x,y)}{f_{y}(x,y)}=\frac{u_{x}}{u_{y}}$
	- Thus the directional derivative is maximized and minimized when the ratio of the direction vector components is the same as that of the components of the gradient
	- This is intuitive if we express the components of the direction vector as a function that forms a half circle (ie. is non-linear) on $x \in[-1,1]$ where $y=f(x)=\sqrt{1-x^2}$ and $f'(x)=-\frac{x}{\sqrt{1-x^2}}$
		- This is why simply taking $\mathbf{i}$ or $\mathbf{j}$ as the direction by whichever partial in the gradient is greater **does not maximize** the directional derivative; the derivative of $y$ with respect to $x$ near $x=\pm1$ goes to $\pm \infty$ (ie. small change in $x$ yields relatively large change in $y$)

We can also rewrite the dot product expression of the directional derivative with the geometric identity of the dot product; if $\theta$ is the angle between $\nabla f$ and $\mathbf{u}$, and noting that the direction vector $\mathbf{u}$ is a unit vector, we get:
$$
\begin{align}
f_{\mathbf{u}}(x,y)=\nabla f(x,y)\cdot \mathbf{u}&=||\nabla f(x,y)||\;||\mathbf{u}||\;\cos\theta \\
&=||\nabla f(x,y)||\cos\theta
\end{align}
$$
From this we can again deduce the relationship between the directional derivative and the gradient:
- $f_{\mathbf{u}}(x,y)=||\nabla f(x,y)||$ is the **maximum**, and occurs when $\cos\theta=1$ so $\theta=0$; when $\nabla f$ and $\mathbf{u}$ point in the exact **same** direction
- $f_{\mathbf{u}}(x,y)=-||\nabla f(x,y)||$ is the **minimum**, and occurs when $\cos\theta=-1$ so $\theta=\pi$; when $\nabla f$ and $\mathbf{u}$ point in the exact **opposite** directions
- $f_{\mathbf{u}}(x,y)=0$ suggests the direction vector is pointing in a direction which would keep you **along the same level curve** of the surface, and occurs when $\cos\theta=0$ so $\theta=\pm\frac{\pi}{2}$; when $\nabla f$ and $\mathbf{u}$ are perpendicular
	- Then we have $\nabla f(x,y)\cdot \mathbf{u}=0$; in the $xy$-plane there are always two such direction vectors pointing in exactly opposite directions, which is intuitive as left and right along a level curve
The gradient vector **points in the direction of the greatest rate of change at a point** and the **magnitude of the gradient vector is that rate of change** (recall though, the gradient is a vector in $\mathbb{R}^2$)
- Recall for differentiable $f$, the contour diagram begins to resemble linear contours as we zoom in by local linearity; thus, the gradient points in the direction **perpendicular to the contour** towards the next higher value, since that is the shortest distance / highest rate of change to the next contour
	- This also means that the **magnitude of the gradient is larger between contours that are closer together**, and smaller between contours farther apart


All concepts above extend directly to three or more variable functions. Specifically for three variables, gradient vectors are lines in $\mathbb{R}^3$, and are **perpendicular to the level surface** at each given point
- Thus, the gradients here are **normal vectors** to **tangent planes**; with the gradient and given point we can substitute into the dot product with the normal vector form of a plane to determine the tangent plane, ie. $f_{x}(a,b,c)(x-a)+f_{y}(a,b,c)(y-b)+f_{z}(a,b,c)(z-c)=0$ and its simplified form
	- This is the $\mathbb{R}^3$ extension of the above concept of gradients being perpendicular to contours (and now surfaces), and of direction vectors which are perpendicular to the gradient being along level surfaces (in $\mathbb{R}^3$ these are all direction vectors on the tangent plane to which the gradient is normal)
### <u>[14.8]: Differentiability</u>
Consider how we can reinterpret the definition of differentiability for single variable functions:
$$
f'(a)=\lim_{ x \to a }\frac{f(x)-f(a)}{x-a}\iff 0=\lim_{ x \to a }\frac{f(x)-[f(a)+f'(a)(x-a)]}{x-a}\iff 0=\lim_{ x \to a }\frac{f(x)-L(a)}{x-a} 
$$
Then the numerator is the error term of the linear approximation around $a$ where $E(a)=f(x)-L(a)$, and the entire term implies that $f$ is differentiable at $a$ when this error goes to $0$ faster than the distance between $x$ and $a$.

A function of two variables $f$ is **differentiable** at $(a,b)$ if there **exists a linear function** $L(x,y)=f(a,b)+m(x-a)+n(y-b)$ such that the error $E(x,y)=f(x,y)-L(x,y)$ satisfies:
$$
\lim_{h,k\to 0}\frac{E(a+h,b+k)}{\sqrt{ h^2 +k^2 }}=\frac{f(a+h,b+k)-(f(a,b)+mh+nk)}{\sqrt{ h^2 +k^2 }}=\frac{(f(a+h,b+k)-f(a,b))-(mh+nk)}{\sqrt{ h^2 +k^2 }}=0 
$$
This limit means that we require **the error to go to** $0$ **faster than the distance**, as we move from $(a+h,b+k)$ to $(a,b)$. If this condition is satisfied, then the function $L$ is the aforementioned local linearization of $f$ near $(a,b)$. We can prove this using the limit above and holding $h,k=0$ (one at a time respectively), to show that $m=f_{x}(a,b)$ and $n=f_{y}(a,b)$ and thus:
$$
\lim_{h,k\to 0}\frac{E(a+h,b+k)}{\sqrt{ h^2 +k^2 }}=\frac{(f(a+h,b+k)-f(a,b))-(f_{x}(a,b)h+f_{y}(a,b)k)}{\sqrt{ h^2 +k^2 }}=\frac{\Delta f-df}{\sqrt{ h^2 +k^2 }}=0 
$$
This also means the function is **smooth** around $(a,b)$ meaning it is differentiable and thus continuous. As was the case with single variable functions: 
- Differentiability$\implies$continuity
- Continuity$\centernot\implies$differentiability 
A function is differentiable on as region $R$ if it is differentiable at all points on $R$. 

The above also means that a differentiable function at a point **necessarily has both partial derivatives** at that point:
- Differentiability$\implies f_{x}(a,b), f_{y}(a,b)$ exist
- Note the converse is not necessarily true; both partial derivatives existing is **does not necessarily imply** differentiability (**nor** continuity by extension)!
Then, to show that a function is **not differentiable** at a point $(a,b)$, we can:
- Show that the relative error limit above is not $0$; this is often impractical, the methods below are used instead
	- Show the contrapositive; that **one or both partial derivatives do not exist**, usually from the definition of partial derivatives, especially with piecewise function
		- In simpler cases it may be possible be possible to simply determine the partial derivative functions and show that they are undefined at some points, etc.
	- Show that the function is **not continuous** at $(a,b)$; recall, to do this it suffices to show **one path** (usually $x=y$ is easiest) with which the limit of the function at $(a,b)$ is not equal to $f(a,b)$

There is a simpler definition; if the partial derivatives $f_{x},f_{y}$ **exist and are continuous** on a small disk centered at the point $(a, b)$, then $f$ is **differentiable** at $(a, b)$
- This class of functions is termed $C^1$; we can use this theorem to prove that functions are differentiable on some required domain $R$ by determining the partial derivative functions and showing that they are continuous on $R$
	- This is another definition of **smoothness**; recall the other definition which gave us $C^2$
- Again, note the converse is not necessarily true; a **differentiable function can have discontinuous partials** at a point
- Intuitively, differentiability at a point $(a,b)$ is defined as the existence of a tangent plane to the surface at that point (following from the existence of continuous partials); similar to the single variable definition with the existence of a tangent line
Then, to show that a function is differentiable, we can:
- Show that both partial derivatives exist and are continuous 
### <u>14.7: Extrema</u>
**Fermat's Theorem:** if $f(x,y)$ has a local maximum or local minimum at a point $(a,b)$ and is **differentiable** there (recall, this means $f_{x}(a,b)$ and $f_{y}(a,b)$ exist and are continuous), then $f_{x}(a,b)=f_{y}(a,b)=0$
- This can also be written in **gradient** form as $\nabla f(a,b)=\mathbf{0}$
	- It follows from the definition of directional derivative for **differentiable** functions that **all directional derivatives** are also $0$ at extrema
	- If the one or both of the partial derivatives do not exist at the point $(a,b)$, then the function is not differentiable at that point and we cannot make any conclusions about the existence or values of various directional derivatives (would have to use definition of directional derivative)
- This implies that the tangent plane at point $(a,b)$ is a **horizontal plane**, since substituting $f_{x}(a,b)=f_{y}(a,b)=0$ into the equation of a plane yields the constant $f(a,b)$

Similar to single variable functions, points where both $f_{x}(a,b)=f_{y}(a,b)=0$ **or** one or both partials **does not exist**, are called **critical points**
- Combined with Fermat's Theorem above, this means **all local extrema are critical points**, but (as it was with single variable functions, for ex. $x^3$ at $0$) **not all critical points are local extrema**!
If a point $(a,b)$ satisfies $f_{x}(a,b)=f_{y}(a,b)=0$ but is **not** an extrema, then it is a **saddle point**
- One clear example is $f(x,y)=y^2-x^2$ and the point $f(0,0)$; this a critical point based on the gradient, but around any disk with a positive radius centred on $(0,0)$, $f$ will take on both positive and negative values, ie. it crosses the horizontal tangent plane at $f(0,0)$ (in this case the $xy$-plane)
	- Thus, this point cannot be an extrema, and indeed the graph in this case resembles a saddle (this is not always the case)

Once we have **identified critical points** by finding points $(a,b)$ where $f_{x}(a,b)=f_{y}(a,b)=0$ or DNE, how do we determine if they are local extrema?

**Second Derivatives Test:** suppose the **second order** partial derivatives of $f$ are **continuous** on a disk centred around a **critical point** $(a,b)$, then define:
$$
D=D(a,b)=(f_{xx}(a,b))(f_{yy}(a,b))-(f_{xy}(a,b))^2
$$
Then:
- If $D>0$ and $f_{xx}(a,b)>0$, then $f(a,b)$ is a **local minimum**
- If $D>0$ and $f_{xx}(a,b)<0$, then $f(a,b)$ is a **local maximum**
- If $D<0$, then $f(a,b)$ is a **saddle point**
- If $D=0$, then we cannot make a conclusion

If **any partials do not exist**, then we **cannot** use the Second Derivatives Test to classify the critical point; we must analyze the function's behaviour around the critical point directly; usually via analyzing both cross sections
- If either cross section crosses between less than and greater than $f(a,b)$, or if one cross section has a local minimum at the point while another has a local maximum; this suggests a saddle point
- If both cross sections are less than or greater than $f(a,b)$ around the point; this suggests local extrema

One common application of finding local extrema is **determining the shortest distance** between a point $(a,b,c)$ and a plane, which involves finding a point $(x,y,z)$ on the plane. The distance is $d=\sqrt{(x-a)^2+(y-b)^2+(z-c)^2}$, we then express $z$ as a function of $x$ and $y$ (from the given plane equation) and thus get $d^2=f(x,y)$, from where we do the second derivatives test process above.
## <u>15: ?</u>
### <u>[15.2]: Global Extrema and Optimization</u>
If we restrict the domain of a two variable function, the resulting domain can be:
- **Closed**; meaning the region is one which contains its boundary
- **Bounded**; meaning the region does not stretch to infinity in any direction
Regions of $\mathbb{R}^2$ can be bounded but not closed, bounded and closed, or neither

**Extreme Value Theorem for Multi Variable Functions:** if $f$ is a **continuous** function on a **closed and bounded** region $R\subset \mathbb{R}^2$, then $f$ has **both** a global maximum and a global minimum at some points in $R$
- If $f$ is not continuous over $R$, or $R$ is not closed and bounded, then EVT cannot guarantee the existence of either global extremum
- This is analogous to the single variable EVT, wherein a continuous function on a closed interval ($\mathbb{R}^2$ analog is a bounded and closed region) is guaranteed to have both global extrema, but no guarantee otherwise

To locate **global extrema** for a function multivariable function on a region of its domain $R\subset \mathbb{R}^2$:
- Find all critical points of $f$ in the region $R$; ie. $(a,b)\in R$ where $f_{x}(a,b)=0$ and $f_{y}(a,b)=0$ or undefined
- Evaluate $f$ at the critical points, or classify by the Second Derivatives Test if needed
- If the region $R$ is **closed**, evaluate $f$ for extrema along the boundaries; ie. consider the boundaries as cross-sections and determine the extrema for the resulting single variable functions
	- Recall here that critical points of a single variable function $g$ occur when $g'(a)=0$ or undefined; sometimes even this is not needed as the function is monotonically increasing or decreasing
- If $R$ is **not closed**, then we need to check the limits of the function as we approach the boundaries (recall how multivariable limits work; hold some variable as a constant $c$ (since picking one specific value may not suffice) and move the relevant variable)
- If $R$ is **not bounded**, then we need to check the limits of the function as we approach infinities (same as above)
### <u>[15.3]: Constrained Optimization</u>
To optimize $f(x, y)$ subject to a constraint $g(x, y) = k$ for some constant $k\in \mathbb{R}$, we want to find the intersection points with the largest $f$ contour value on the graph of the $g$ contour $g(x, y) = k$
- By definition of contours, extrema should occur at points where the contours of $f$ are tangent to $g(x, y) = k$; ie. at an extremum $(a,b)$, we should have $\nabla f(a,b)=\lambda\nabla g(a,b)$ for some constant $\lambda \in \mathbb{R}$ called the **Lagrange multiplier**
	- The Lagrange multiplier is the **rate of change** of $f$ per unit increase in the level of the constraint function $g$

However, $\nabla g$ may not always be well defined and nonzero; if $f(x,y)$ has an extrema subject to a constraint $g(x,y) = k$ at $(a,b)$, then $(a,b)$ satisfies **one** of:
1. $(a,b)$ is an end point of $g(a,b) = k$
2. $\nabla g(a,b) = \mathbf{0}$
3. $\nabla f(a,b) = \lambda\nabla g(a,b)$; this is the happy path we defined above, and the condition that we check last
	- We calculate all the necessary partials then set up the equality above, then substitute back into $g$ to solve for $a,b$ (or use Lagrangian shortcut below); ie. solving a system of **three** equations:
		- $f_{x}=\lambda g_{x}$
		- $f_{y}=\lambda g_{y}$
		- $g(x,y)=k$
We determine all $(a,b)$ that satisfy **any** of the conditions above, and evaluate $f$ to determine the correct extrema

If the constraint $g(x,y)\leq k$ is given by an inequality instead of equality, we use the following strategy:
1. Find all points in the region $g(x, y) < k$ where $\nabla f=\mathbf{0}$ or undefined
2. Find the local extrema of $f$ on the boundary $g(x,y) = k$ using the same process as above
Then evaluate points found in the previous two steps and compare their values as usual

**Lagrangian Function:** $L(x,y,\lambda) = f(x,y)-\lambda(g(x,y)-k)$
- Then $(a,b)$ is an extremum under the constraint $g(x,y)=k$ with Lagrange multiplier $\lambda_{c}$ iff $\nabla L(a,b,\lambda_{c})=0$
	- This gradient gives us a system of equations to solve for $(a,b)$ as a shortcut
	- Note, endpoints of $g$ still have to be checked

If the **constraint function is a closed and bounded** shape (ex. a circle like $x^2 + y^2 = 1$) then EVT guarantees that an absolute maximum and minimum must exist; in this case simply evaluate the objective function $f$ at all the points you found and classify the extrema by direct comparison