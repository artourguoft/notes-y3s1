# <u>Geometry of Linear Programming</u>
Let $\mathbf{a}$ be some **nonzero** vector in $\mathbb{R}^n$ representing the coefficients of a linear equation, and let $s$ be some scalar: 
- **Hyperplane:** the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}'\mathbf{x}=s\}$ defines a hyperplane; the set of points $\mathbf{x}$ that satisfies the equation equaling $s$
	- Hyperplanes are necessarily subspaces, so the dimension $n$ here suggests the ambient space is $\mathbb{R}^{m}$ where $n=m-1$, but note by definition of a subspace they are $m-1$ dimensional objects in $m$ dimensional space; they are not the space $\mathbb{R}^{m-1}$ itself
	- The vector $\mathbf{a}$ is **perpendicular to the hyperplane** itself; consider $\mathbf{x}_{1},\mathbf{x}_{2}$ in the hyperplane, then $\mathbf{a}'\mathbf{x}_{1}=\mathbf{a}'\mathbf{x}_{2}$ from which $\mathbf{a}'(\mathbf{x}_{1}-\mathbf{x}_{2})=0$
	- Examples:
		- In $\mathbb{R}$ a hyperplane is a **point** 
		- In $\mathbb{R}^2$ a hyperplane is a **line** where with $a_{1},a_{2},s\in \mathbb{R}$ and $\mathbf{x}\in \mathbb{R}^2$, the line is $a_{1}x_{1}+a_{2}x_{2}=s$ or $\mathbf{a}'\mathbf{x}=s$
			- For a **horizontal line**, $a_{1}=0$ since $x_{1}$ is the $x$-axis, and all points with $x_{2}=\frac{s}{a_{2}}$ are solutions
			- For a **vertical line**, $a_{2}=0$ since $x_{2}$ is the $y$-axis, and all points with $x_{2}=\frac{s}{a_{1}}$ are solutions
		- In $\mathbb{R}^3$ a hyperplane is a **plane** where with $a_{1},a_{2},a_{3},s\in \mathbb{R}$ and $\mathbf{x}\in \mathbb{R}^3$, the line is $a_{1}x_{1}+a_{2}x_{2}+a_{3}x_{3}=s$ or $\mathbf{a}'\mathbf{x}=s$ 
			- For a **horizontal plane**, $a_{1},a_{2}=0$ since $x_{1},x_{2}$ are the $x$ and $y$ axes, and all points with $x_{3}=\frac{s}{a_{3}}$ are solutions
- **Halfspace:** the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}'\mathbf{x}\geq s\}$ defines a halfspace; the set of points $\mathbf{x}$ that satisfies the equation being $\geq s$
	- Note, this is a **closed halfspace**, the other half of which is the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}'\mathbf{x}\leq s\}$
	- We also define an **open halfspace** as the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}'\mathbf{x}> s\}$, and the other half of which is $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}'\mathbf{x}< s\}$
	- The hyperplane is the boundary of its corresponding halfspaces; for closed halfspaces this boundary is also contained in the halfspace
  
We can also define a line by any **two distinct points** $\mathbf{x}_{1}\neq\mathbf{x}_{2}$ on the line, as the set $\{ \mathbf{x}\in \mathbb{R}^n :\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\text{ where }\lambda \in \mathbb{R}\}$
- Note $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}=\mathbf{x}_{2}+\lambda(\mathbf{x}_{1}-\mathbf{x}_{2})$, so the whole line is all scalar multiples of the line segment connecting the two points
- With a modification to the range of $\lambda$ above to $\{ \mathbf{x}\in \mathbb{R}^n :\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\text{ where }\lambda \in [0,1]\}$, we now have $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}$ by definition being a weighted average of $\mathbf{x}_{1},\mathbf{x}_{2}$ and thus representing points along $\mathbf{x}_{2}-\mathbf{x}_{1}$ which is **only** the line segment connecting the two points
  
A set $S\subset \mathbb{R}^n$ is a **convex set** if for all $\mathbf{x}_{1}\neq\mathbf{x}_{2}\in S$ and any $\lambda \in[0,1]$, we have $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\in S$
- Thus the geometric interpretation is that a set is convex if any segment connecting any two of its elements is also contained entirely in the set
- Some important conclusions regarding interplay between convexity and linearity, along with their proofs (with previous assumptions on $\mathbf{x}_{1},\mathbf{x}_{2}$ and $\lambda$):
	- Trivially, **lines** are convex directly from the definition with two distinct points, and more generally:
	- **Hyperplanes** are convex sets:
		- Define a hyperplane $H:=\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}'\mathbf{x}=s\}$ and let $\mathbf{x}_{1},\mathbf{x}_{2}\in H$; then $\lambda (\mathbf{a}'\mathbf{x}_{1})+(1-\lambda)(\mathbf{a}'\mathbf{x}_{2})=\lambda (s)+(1-\lambda)(s)=s$ as needed, by assumptions on $\mathbf{x}_{1},\mathbf{x}_{2}$ and on $\lambda$
	- **Halfspaces** are convex sets:
		- Define a closed halfspace $H:=\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}'\mathbf{x}\geq s\}$ and let $\mathbf{x}_{1},\mathbf{x}_{2}\in H$; then $\lambda (\mathbf{a}'\mathbf{x}_{1})+(1-\lambda)(\mathbf{a}'\mathbf{x}_{2})\geq\lambda (s)+(1-\lambda)(s)=s$ as needed, by assumptions on $\mathbf{x}_{1},\mathbf{x}_{2}$ and on $\lambda$ (and by direction of inequality being preserved since both $\lambda,1-\lambda\geq 0$)
			- Same proofs follow for the other side of the halfspace, and for open halfspaces
	- The **intersection of convex sets** is convex:
		- Define convex sets $S_{1},\dots,S_{k}\in \mathbb{R}^n$ and let $\mathbf{x}_{1},\mathbf{x}_{2}\in \bigcap_{i=1}^kS_{i}$; then since each $S_{i}$ is convex, for all $i$ we have $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\in S_{i}$ which is directly the definition of $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\in \bigcap_{i=1}^kS_{i}$

Let $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$ be vectors in $\mathbb{R}^n$ and let $a_{1},\dots,a_{k}\in \mathbb{R}_{\geq 0}$ and $\sum_{i=1}^ka_{i}=1$:
- Then the vector sum $\sum_{i=1}^ka_{i}\mathbf{x}_{i}$ is a **convex combination** of the vectors $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$
- The set of **all** such convex combinations $\{ \sum_{i=1}^ka_{i}\mathbf{x}_{i} : a_{1},\dots,a_{k}\in \mathbb{R}_{\geq 0}\wedge \sum_{i=1}^ka_{i}=1\}$ defines the **convex hull** of the vectors $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$
	- The convex hull of the vectors $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$ is the **smallest convex set containing** all of $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$, which means any convex set which contains $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$ must contain the convex hull
- A **convex polytope** is a convex hull of **finitely** many vectors
	- Convex polytopes are convex by definition, and they are also **bounded** meaning there exist constants such that every component of every element of the polytope is less than or equal to the corresponding constant; in $\mathbb{R}^2$ and $\mathbb{R}^3$ this is easy to visualize as polytopes being bounded by some **rectangles**
	- **Rectangle:** a rectangle in $\mathbb{R}^n$ is a set of the form $R:=\{ \mathbf{x}\in \mathbb{R}^n:a_{i}\leq x_{i}\leq b_{i}\text{ where }1\leq i\leq n \}$ meaning for each dimension there is a pair of constants bounding that component of every contained vector
		- Rectangles are convex polytopes; they are formed by all convex combinations of their finitely many vertices and they are bounded by definition
		- Formally we define a set $S\subseteq \mathbb{R}^n$ as **bounded** if it can be contained in a rectangle, and **unbounded** if it cannot be contained in any rectangle

**Polyhedron:** a set that can be described as $\bigcap_{i=1}^m \{ \mathbf{x}\in \mathbb{R}^n:\mathbf{a}_{i}'\mathbf{x}\geq b_{i}\}=\{ \mathbf{x}\in \mathbb{R}^n : \mathbf{A}\mathbf{x}\geq \mathbf{b}\}$ where $\mathbf{A}$ is an $m\times n$ matrix and $\mathbf{b}\in \mathbb{R}^m$
- So polyhedrons are **intersections of finitely many closed halfspaces**, and are sets of solutions to systems of $m$ linear inequalities
	- Recall, halfspaces are convex sets, and the intersection of convex sets is convex, therefore; **polyhedrons are convex sets** and we will refer to them as convex polyhedrons henceforth
- The simplest convex polyhedron is a single closed halfspace where $m=1$
- Convex polyhedrons that are **bounded** are by definition convex polytopes

A point in a convex set $S\subseteq \mathbb{R}^n$ is an **extreme point** if its **not an interior point** of any line segment contained in $S$
- The geometric intuition; you cannot draw a line segment that is entirely contained in $S$, in which an extreme point is contained in the interior
- Formally a point $\mathbf{x}$ is an extreme point if $\neg \exists \mathbf{x}_{1},\mathbf{x}_{2}\in S,\mathbf{x}=\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}$ for any $\lambda \in(0,1)$
- Ex. the unit sphere encasing the unit ball $\{ \mathbf{x}\in \mathbb{R}^3:||\mathbf{x}||\leq 1\}$ is made entirely of extreme points of the ball

**Convex Function:** a function $f:S\subseteq \mathbb{R}^n\to \mathbb{R}$ defined on a convex set $S$ is a convex function if for all $\mathbf{x}_{1},\mathbf{x}_{2}\in S$ and $\lambda \in[0,1]$: $f(\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2})\leq \lambda f(\mathbf{x}_{1})+(1-\lambda)f(\mathbf{x}_{2})$
- Then consider a standard linear function $f:\mathbb{R}^n\to \mathbb{R}$ of the form $f(\mathbf{x})=\mathbf{a}'\mathbf{x}$ is convex, directly from the definition above
# <u>Linear Programming Problems</u>
A
