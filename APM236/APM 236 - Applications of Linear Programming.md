# <u>Geometry of Linear Programming</u>
Let $\mathbf{a}$ be some **nonzero** vector in $\mathbb{R}^n$ representing the coefficients of a linear equation, and let $s$ be some scalar: 
- **Hyperplane:** the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}^{\top}\mathbf{x}=s\}$ defines a hyperplane; the set of points $\mathbf{x}$ that satisfies the equation equaling $s$
	- Hyperplanes are necessarily subspaces (linear or affine), so the dimension $n$ here suggests the ambient space is $\mathbb{R}^{m}$ where $n=m-1$, but note by definition of a subspace they are $m-1$ dimensional objects in $m$ dimensional space; they are not the space $\mathbb{R}^{m-1}$ itself
	- The vector $\mathbf{a}$ is **perpendicular to the hyperplane** itself; consider $\mathbf{x}_{1},\mathbf{x}_{2}$ in the hyperplane, then $\mathbf{a}^{\top}\mathbf{x}_{1}=\mathbf{a}^{\top}\mathbf{x}_{2}$ from which $\mathbf{a}^{\top}(\mathbf{x}_{1}-\mathbf{x}_{2})=0$
	- Examples:
		- In $\mathbb{R}$ a hyperplane is a **point** 
		- In $\mathbb{R}^2$ a hyperplane is a **line** where with $a_{1},a_{2},s\in \mathbb{R}$ and $\mathbf{x}\in \mathbb{R}^2$, the line is $a_{1}x_{1}+a_{2}x_{2}=s$ or $\mathbf{a}^{\top}\mathbf{x}=s$
			- For a **horizontal line**, $a_{1}=0$ since $x_{1}$ is the $x$-axis, and all points with $x_{2}=\frac{s}{a_{2}}$ are solutions
			- For a **vertical line**, $a_{2}=0$ since $x_{2}$ is the $y$-axis, and all points with $x_{1}=\frac{s}{a_{1}}$ are solutions
		- In $\mathbb{R}^3$ a hyperplane is a **plane** where with $a_{1},a_{2},a_{3},s\in \mathbb{R}$ and $\mathbf{x}\in \mathbb{R}^3$, the line is $a_{1}x_{1}+a_{2}x_{2}+a_{3}x_{3}=s$ or $\mathbf{a}^{\top}\mathbf{x}=s$ 
			- For a **horizontal plane**, $a_{1},a_{2}=0$ since $x_{1},x_{2}$ are the $x$ and $y$ axes, and all points with $x_{3}=\frac{s}{a_{3}}$ are solutions
- **Halfspace:** the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}^{\top}\mathbf{x}\geq s\}$ defines a halfspace; the set of points $\mathbf{x}$ that satisfies the inequality being $\geq s$
	- Note, this is a **closed halfspace**, the other half of which is the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}^{\top}\mathbf{x}\leq s\}$
	- We also define an **open halfspace** as the set $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}^{\top}\mathbf{x}> s\}$, and the other half of which is $\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}^{\top}\mathbf{x}< s\}$
	- The hyperplane is the boundary of its corresponding halfspaces; for closed halfspaces this boundary is also contained in the halfspace
	- Note, halfspaces open in the **opposite direction** of $\mathbf{a}$
  
We can also define a line by any **two distinct points** $\mathbf{x}_{1}\neq\mathbf{x}_{2}$ on the line, as the set $\{ \mathbf{x}\in \mathbb{R}^n :\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\text{ where }\lambda \in \mathbb{R}\}$
- Note $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}=\mathbf{x}_{2}+\lambda(\mathbf{x}_{1}-\mathbf{x}_{2})$, so the whole line is all scalar multiples of the line segment connecting the two points
- With a modification to the range of $\lambda$ above to $\{ \mathbf{x}\in \mathbb{R}^n :\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\text{ where }\lambda \in [0,1]\}$, we now have $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}$ by definition being a weighted average of $\mathbf{x}_{1},\mathbf{x}_{2}$ and thus representing points along $\mathbf{x}_{2}-\mathbf{x}_{1}$ which is **only the line segment connecting** the two points
  
A set $S\subset \mathbb{R}^n$ is a **convex set** if for all $\mathbf{x}_{1}\neq\mathbf{x}_{2}\in S$ and any $\lambda \in[0,1]$, we have $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\in S$
- Thus the geometric interpretation is that a set is convex if any segment connecting any two of its elements is also contained entirely in the set
- Some important conclusions regarding interplay between convexity and linearity, along with their proofs (with previous assumptions on $\mathbf{x}_{1},\mathbf{x}_{2}$ and $\lambda$):
	- Trivially, **lines** are convex directly from the definition with two distinct points, and more generally:
	- **Hyperplanes** are convex sets:
		- Define a hyperplane $H:=\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}^{\top}\mathbf{x}=s\}$ and let $\mathbf{x}_{1},\mathbf{x}_{2}\in H$; then $\lambda (\mathbf{a}^{\top}\mathbf{x}_{1})+(1-\lambda)(\mathbf{a}^{\top}\mathbf{x}_{2})=\lambda (s)+(1-\lambda)(s)=s$ as needed, by assumptions on $\mathbf{x}_{1},\mathbf{x}_{2}$ and on $\lambda$
	- **Halfspaces** are convex sets:
		- Define a closed halfspace $H:=\{ \mathbf{x}\in \mathbb{R}^n :\mathbf{a}^{\top}\mathbf{x}\geq s\}$ and let $\mathbf{x}_{1},\mathbf{x}_{2}\in H$; then $\lambda (\mathbf{a}^{\top}\mathbf{x}_{1})+(1-\lambda)(\mathbf{a}^{\top}\mathbf{x}_{2})\geq\lambda (s)+(1-\lambda)(s)=s$ as needed, by assumptions on $\mathbf{x}_{1},\mathbf{x}_{2}$ and on $\lambda$ (and by direction of inequality being preserved since both $\lambda,1-\lambda\geq 0$)
			- Same proofs follow for the other side of the halfspace, and for open halfspaces
	- The **intersection of convex sets** is convex:
		- Define convex sets $S_{1},\dots,S_{k}\in \mathbb{R}^n$ and let $\mathbf{x}_{1},\mathbf{x}_{2}\in \bigcap_{i=1}^kS_{i}$; then since each $S_{i}$ is convex, for all $i$ we have $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\in S_{i}$ which is directly the definition of $\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}\in \bigcap_{i=1}^kS_{i}$
	- The **set of solutions** $\mathbf{x}$ to a system of linear equations is convex:
		- Define $S:=\{ \mathbf{x}\in \mathbb{R}^n: \mathbf{A}\mathbf{x}=\mathbf{b}\}$ where $\mathbf{A}\in \mathbb{R}^{m\times n}$ and $\mathbf{b}\in \mathbb{R}^m$; let $\mathbf{x}_{1},\mathbf{x}_{2}\in S$, then $\lambda(\mathbf{x}_{1})+(1-\lambda)(\mathbf{x}_{2})\in \mathbb{R}^n$ from which $\mathbf{A}\cdot(\lambda(\mathbf{x}_{1})+(1-\lambda)(\mathbf{x}_{2}))=\lambda\mathbf{A}\mathbf{x}_{1}+(1-\lambda)\mathbf{A}\mathbf{x}_{2}=\lambda \mathbf{b}+(1-\lambda)\mathbf{b}=\mathbf{b}$ as needed
		- This also applies to sets of solutions to systems of linear inequalities, with a similar proof

Let $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$ be vectors in $\mathbb{R}^n$ and let $a_{1},\dots,a_{k}\in \mathbb{R}_{\geq 0}$ and $\sum_{i=1}^ka_{i}=1$: 
- **Convex Combination:** the vector sum $\sum_{i=1}^ka_{i}\mathbf{x}_{i}$ is a convex combination of the vectors $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$
- **Convex Hull:** the set of **all** (ie. all permutations of $a_{i}$) such convex combinations $\{\mathbf{x}\in \mathbb{R}^n: \mathbf{x}=\sum_{i=1}^ka_{i}\mathbf{x}_{i}\wedge a_{1},\dots,a_{k}\in \mathbb{R}_{\geq 0}\wedge \sum_{i=1}^ka_{i}=1\}$ is the **convex hull** of the vectors $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$
	- The convex hull of the vectors $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$ is the **smallest convex set containing** all of $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$, which means that any convex set which contains $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$ must contain the convex hull; and that adding more vectors to $\mathbf{x}_{1},\dots,\mathbf{x}_{k}$ will **never** make the convex hull **smaller** (although interior points can be redundant)
- **Convex Polytope:** a convex hull of **finitely** many vectors
	- Convex polytopes are convex by definition, and they are also **bounded** meaning there exists a constants such that every component of every element of the polytope is less than or equal to the corresponding constant; in $\mathbb{R}^2$ and $\mathbb{R}^3$ this is easy to visualize as polytopes being bounded by some **rectangles** (although this applies to all dimensions!)
	- **Rectangle:** a rectangle in $\mathbb{R}^n$ is a set of the form $R:=\{ \mathbf{x}\in \mathbb{R}^n:a_{i}\leq x_{i}\leq b_{i}\text{ where }1\leq i\leq n \}$ meaning for each dimension there is a pair of constants bounding that component of every contained vector
		- Rectangles are convex polytopes; they are formed by all convex combinations of their finitely many vertices and they are bounded by definition
		- Formally we define a set $S\subseteq \mathbb{R}^n$ as **bounded** if it can be contained in a rectangle, and **unbounded** if it cannot be contained in any rectangle

**Polyhedron:** a set that can be described as $\bigcap_{i=1}^m \{ \mathbf{x}\in \mathbb{R}^n:\mathbf{a}_{i}^{\top}\mathbf{x}\leq b_{i}\}=\{ \mathbf{x}\in \mathbb{R}^n : \mathbf{A}\mathbf{x}\leq \mathbf{b}\}$ where $\mathbf{A}$ is an $m\times n$ matrix and $\mathbf{b}\in \mathbb{R}^m$
- So polyhedrons are **intersections of finitely many closed halfspaces** ($m$ halfspaces), and contain solutions to systems of $m$ linear inequalities
	- Recall, halfspaces are convex sets, and the intersection of convex sets is convex, therefore; **polyhedrons are convex sets** and we will refer to them as convex polyhedrons henceforth
- The simplest convex polyhedron is a single closed halfspace, so where $m=1$
- If a convex polyhedrons is **bounded** then by definition it is also a convex polytope defined by **all convex combinations of its finitely many extreme points** (more on extreme points later); it if its unbounded (ex. a single closed halfspace) then it is still a convex hull by definition but not a convex polytope

**Convex Function:** a function $f:S\subseteq \mathbb{R}^n\to \mathbb{R}$ defined on a convex set $S$ is a convex function if for all $\mathbf{x}_{1},\mathbf{x}_{2}\in S$ and $\lambda \in[0,1]$ we have $f(\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2})\leq \lambda f(\mathbf{x}_{1})+(1-\lambda)f(\mathbf{x}_{2})$
- Then consider that a standard **linear function** $f:\mathbb{R}^n\to \mathbb{R}$ of the form $f(\mathbf{x})=\mathbf{a}^{\top}\mathbf{x}$ is convex, directly from the definition above
- Let $f_{1},f_{2},\dots,f_{k}:\mathbb{R}^n\to \mathbb{R}$ be convex linear or affine functions, and let $g$ be a **piecewise linear convex function** defined over various intervals by these functions, then:
	- The function $g(\mathbf{x})=\max(f_{1},f_{2},\dots,f_{k})$ is **also a convex function**, and **minimizing** said function is the same as solving a standard LPP where each function is a constraint $f_{i}=\mathbf{a}^{\top}\mathbf{x}\leq z$ and we seek to minimize $-\max(-z)$ where $z=g(\mathbf{x})$, ie. the smallest value that the maximum of all the functions hits
		- Then, each halfspace opens upward in the $z$ direction (set $f_{i}=\mathbf{a}^{\top}\mathbf{x}-z\leq 0$, then the $z$ component is $-1$)
		- Since we are minimizing, recall we negate the maximum of the negation of the original objective, and we must also constrain $z=z^+ -z^-$ where $z^+,z^- \geq 0$
	- Note, this theorem also applies for nonlinear $f_{1},f_{2},\dots,f_{k}:\mathbb{R}^n\to \mathbb{R}$, but
	- Also note that piecewise linear convex functions can **approximate any convex function**
# <u>Linear Programming Problems</u>
**General Form:** find values of $x_{1},x_{2},\dots,x_{n}$ such that:
$$
\begin{aligned}
\text{max or min}\quad z=\mathbf{c}^{\top}\mathbf{x} \\
\mathbf{a}_{1}^{\top}\mathbf{x} \le b_{1} \\
\mathbf{a}_{2}^{\top}\mathbf{x} \ge b_{2} \\
\vdots \\
\mathbf{a}_{m}^{\top}\mathbf{x} = b_{m} \\
\mathbf{x} \ge \mathbf{0}
\end{aligned}
$$
Where each equality or inequality in the constraints chooses **any one** of the signs
- The **objective function** is the function to maximize or minimize
- The **constraints** are the rest of the equalities or inequalities to be satisfied
- The **variables are all constrained** to be nonnegative as well, ie. $\mathbf{x}\geq 0$
- The objective function and constraints can also be expanded out into linear summations
	- The constraints can be compacted into $\mathbf{A}\mathbf{x}=\mathbf{b}$ if the sign is the same among all constraints (equality here), where $\mathbf{A}\in \mathbb{R}^{m\times n}$, $\mathbf{x}\in \mathbb{R}^n$ and $\mathbf{b}\in \mathbb{R}^m$
A vector that satisfies the constraints of an LPP is a **feasible solution**, the set of all feasible solutions is the **feasible set**, and the feasible solution that maximizes or minimizes the objective function as needed is the **optimal solution**

**Standard Form:**
$$
\begin{aligned}
\text{max}\quad z=\mathbf{c}^{\top}\mathbf{x} \\
\mathbf{A}\mathbf{x} \le \mathbf{b} \\
\mathbf{x} \ge \mathbf{0}
\end{aligned}
$$
**Canonical Form:**
$$
\begin{aligned}
\text{max}\quad z=\mathbf{c}^{\top}\mathbf{x} \\
\mathbf{A}\mathbf{x} = \mathbf{b} \\
\mathbf{x} \ge \mathbf{0}
\end{aligned}
$$
We do not have a standard or canonical form if we have an LPP where:
- The objective function is to be minimized
- Some variables are not constrained to be nonnegative
- The constraints have varying signs

However, we can show that **any arbitrary LPP has an equivalent standard form**:
- Every minimization objective can be turned into a maximization objective, by observing that $\min\sum_{i=1}^nc_{i}x_{i}=-\max(-\sum_{i=1}^nc_{i}x_{i})$
- Any $\geq$ inequality $a_{1}x_{1}+a_{2}x_{2}+\dots+a_{n}x_{n}\geq b$ can simply be negated to get $-a_{1}x_{1}-a_{2}x_{2}-\dots-a_{n}x_{n}\leq -b$
- Any equality can be written as two inequalities, ie. $\sum_{i=1}^na_{i}x_{i}=b$ can be written in the constraints as $\sum_{i=1}^na_{i}x_{i}\geq b$ and $\sum_{i=1}^na_{i}x_{i}\leq b$ together
- If any variable $x_{i}$ is unconstrained, define it as $x_{i}=x_{i}^+ -x_{i}^-$ since any real number can be defined as the difference of two nonnegative reals, set $x_{i}^+,x_{i}^- \geq 0$, and update the variable constraints, all affected inequalities, and the objective function with this new expression

Any LPP also has an **equivalent canonical form**:
- In each inequality in the constraints we can introduce a **slack variable** $x_{n+j}$ where $j\in[1,m]$ per inequality, turning them into equalities and making the system of equations in up to $n+m$ unknowns, and introducing additional nonnegativity constraints (on each slack variable)
# <u>The Extreme Point Theorem</u>
The set of all points that satisfies the constraints on an LPP is the **intersection of closed halfspaces and hyperplanes**; recall, both are convex sets and the intersection of convex sets is convex, thus the **feasible set is convex** if it is nonempty
- Specifically, nonempty **feasible sets are convex polyhedrons** defined by all convex combinations of their **finitely many extreme points**, and if the polyhedron is **bounded** then the feasible set is a **convex polytope**

**Extreme Point:** a point $\mathbf{u}$ in a convex set $S\subseteq \mathbb{R}^n$ is an extreme point iff it is **not an interior point of any line segment** contained fully in $S$
- The geometric intuition; you cannot draw a line segment that is entirely contained in $S$, in which an extreme point is contained in the interior
- Formally a point $\mathbf{u}$ is an extreme point if $\neg \exists \mathbf{x}_{1}\neq\mathbf{x}_{2}\in S,\mathbf{u}=\lambda \mathbf{x}_{1}+(1-\lambda)\mathbf{x}_{2}$ for any $\lambda \in(0,1)$, ie. $\mathbf{u}$ is **not** a convex combination of **other points** in $S$ 
- Ex. the unit sphere encasing the unit ball $\{ \mathbf{x}\in \mathbb{R}^3:||\mathbf{x}||\leq 1\}$ is made entirely of extreme points of the ball

**Extreme Point Theorem:** let $S\subseteq \mathbb{R}^n$ be the feasible set of a general LPP
1. **If** $S$ is nonempty and **bounded** (is a convex polytope), **then** an optimal solution exists and occurs at an extreme point
2. **If** $S$ is nonempty and **not bounded** (is a convex polyhedron) and **if** an optimal solution exists, **then** an optimal solution occurs at an extreme point
3. **If** an optimal solution **does not exist**, **then** $S$ is either empty **or** unbounded

Consider why **optimal solutions do not occur at interior points**:
- Let $\mathbf{x}^*\in\{ \mathbf{x}\in \mathbb{R}^n: \mathbf{A}\mathbf{x}\leq\mathbf{b}\}$ be an interior point of this feasible set, that is $\mathbf{A}\mathbf{x}^*<\mathbf{b}$; then by definition of interior points we can perturb $\mathbf{x}^*$ by a chosen direction vector $\mathbf{d}\in \mathbb{R}^n$ scaled by $\epsilon>0$ while remaining in the feasible set, such that $\mathbf{x}^{**}=\mathbf{x}^*+\epsilon \mathbf{d}$ further reduces or increases the objective function
# <u>Basic Solutions</u>
Consider a canonical LPP where $\mathbf{A}\in \mathbb{R}^{m\times n}$, $\mathbf{x}\in \mathbb{R}^n$ and $\mathbf{b}\in \mathbb{R}^m$
- Assume $m\leq n$, and assume that $\text{rank}(\mathbf{A})=m$; that is $\mathbf{A}$ contains at least one set of $m$ **linearly independent columns**
	- If we cannot assume $\text{rank}(\mathbf{A})=m$, then do row-reduction of $\mathbf{A}$ to determine $\text{rank}(\mathbf{A})$ first
- **Any** set of $m$ linearly independent columns of $\mathbf{A}$ forms a **basis** for $\mathbb{R}^m$ which is a basis matrix $\mathbf{B}\in\mathbb{R}^{m\times m}$, then for each $\mathbb{R}^{m}$ column in $\mathbf{B}$ the corresponding column index in $\mathbf{A}$ marks a **basic variable**, with the remaining $n-m$ columns of the original $\mathbf{A}$ marking **nonbasic variables**
	- The nonbasic $x$ values in $\mathbf{x}$ are then set to $0$, from which it follows that at most $m$ components of any extreme point of $S$ can be positive and the rest must be $0$
	- Then $\mathbf{A}\mathbf{x}=\mathbf{b}$ becomes equivalent to $\mathbf{B}\mathbf{x}=\mathbf{b}$, which is a system of $m$ equations in $m$ unknowns and thus has a **unique solution** $\mathbf{x}$ which we solve for by **row-reducing** $[\mathbf{B}|\mathbf{b}]$, and is called a **basic solution**
		- The number of **distinct** basic solutions is **upper bounded** by $\binom{n}{m}$; the number of choices of $m$ columns from $n$ (note $\mathbf{A}$ may have less linearly independent sets of $m$ columns than that)
		- Note, basic solutions are **not necessarily feasible solutions** because they do not account for the $\mathbf{x}\geq 0$ constraint; any basic solution where all components are nonnegative is called a **basic feasible solution** or BFS
			- One **all BFSs** have been identified **truncate** these vectors to remove any possible slack variables, and test the resulting solutions to determine the optimal solution

