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
	- The **set of solutions** $\mathbf{x}$ to a system of linear equalities is convex:
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
	- Note, all slack variables must also be constrained to $x_{n+j}\geq 0$
	- The resulting coefficient matrix $\mathbf{A}$ is in $\mathbb{R}^{m\times(n+m)}$
	- The resulting objective function becomes $f:\mathbb{R}^{m\times(n+m)}\to \mathbb{R}$ where the coefficients of the slack variables are $0$ (but still count for the dimension of the domain!)

==Objectives are functions in dimension of all LPP variables and slack variables (regardless of 0 coefficients)
Absolute values in objectives vs absolute values in constraints
LPPs with variables for coefficients==
# <u>Extreme Point Theorem</u>
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
Consider a **canonical** LPP where $\mathbf{A}\in \mathbb{R}^{m\times n}$, $\mathbf{x}\in \mathbb{R}^n$ and $\mathbf{b}\in \mathbb{R}^m$
- Assume $m\leq n$, and assume that $\text{rank}(\mathbf{A})=m$; that is $\mathbf{A}$ contains at least one set of $m$ **linearly independent columns**
	- If we cannot assume $\text{rank}(\mathbf{A})=m$, then do row-reduction of $\mathbf{A}$ to determine $\text{rank}(\mathbf{A})$ first
- **Any** set of $m$ linearly independent columns of $\mathbf{A}$ forms a **basis** for $\mathbb{R}^m$ which is a basis matrix $\mathbf{B}\in\mathbb{R}^{m\times m}$, then for each $\mathbb{R}^{m}$ column in $\mathbf{B}$ the corresponding column index in $\mathbf{A}$ marks a **basic variable**, with the remaining $n-m$ columns of the original $\mathbf{A}$ marking **nonbasic variables**
	- The nonbasic $x$ values in $\mathbf{x}$ are then set to $0$, from which it follows that $\leq m$ components of any basic solution can be positive and **the rest must be** $0$
	- Then $\mathbf{A}\mathbf{x}=\mathbf{b}$ becomes equivalent to $\mathbf{B}\mathbf{x}=\mathbf{b}$, which is a system of $m$ equations in $m$ unknowns and thus has a **unique solution** $\mathbf{x}$ which we solve for by **row-reducing** $[\mathbf{B}|\mathbf{b}]$, and is called a **basic solution**
		- The number of **distinct** basic solutions is **upper bounded** by $\binom{n}{m}$; the number of choices of $m$ columns from $n$ (so $\mathbf{A}$ may have less linearly independent sets of $m$ columns than this)
		- Note, basic solutions are **not necessarily feasible solutions** because they do not account for the $\mathbf{x}\geq 0$ constraint; any basic solution where all components are nonnegative is called a **basic feasible solution** or BFS
			- One **all BFSs** have been identified **truncate** these vectors to remove any possible slack variables, and test the resulting solutions to determine the optimal solution
Note that this method can **only** be used for LPPs in **canonical** form, but **any optimal solution to the canonical form** found as described above **is an optimal solution to the equivalent standard form**

**Every BFS is an extreme point** of the feasible set (and every extreme point is a BFS), whereas basic solutions that are infeasible are not extreme points since they are not in the feasible set at all
# <u>Simplex Method</u>
**Adjacent:** two extreme points in a set of feasible solutions $S$ are adjacent they have **all but one** in common in their **selection of basic variables**; ie. the selection of nonzero components differs in two places

**Simplex Method:** a method that iterates from a given extreme point to an **adjacent** extreme point such that the value of the objective function increases or remains the same, and proceeds until either an optimal solution is identified or it is determined that the given problem has no finite optimal solution
- Assume $\mathbf{b}\geq 0$ (if not, will have to use the two-phase method, covered later)
- Set all non-slack variables as nonbasic variables, ie. set all non-slack components of $\mathbf{x}$ to $0$; then we are left with **all slack variables as basic variables**, ie. $x_{n+1}=b_{1},x_{n+2}=b_{2},\dots,x_{n+m}=b_{n}$
	- Since $(n+m)-n=m\geq m$ this is a basic solution, and since $\mathbf{b}\geq 0$ this is a feasible solution, and thus this is a BFS
		- Use this BFS to determine an initial value $z_{0}$ for the objective function, then set up the tableau below
$$
\begin{array}{c|ccccccccc|c}
\hline
& x_1 & x_2 & \cdots & x_{n} & x_{n+1} & x_{n+2} & \cdots & x_{n+m} & z & \\
\hline
x_{n+1} & a_{11} & a_{12} & \cdots & a_{1n} & 1 & 0 & \cdots & 0 & 0 & b_{1} \\
x_{n+2} & a_{21} & a_{22} & \cdots & a_{2n} & 0 & 1 & \cdots & 0 & 0 & b_{2}  \\ 
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots \\
x_{n+m} & a_{m1} & a_{m2} & \cdots & a_{mn} & 0 & 0 & \cdots & 1 & 0 & b_{m} \\
\hline
\text{obj}& -c_{1} & -c_{2} & \cdots & -c_{n} & 0 & 0 & \cdots & 0 & 1 & 0 \\
\hline
\end{array}
$$
At this stage we have an initial BFS as determined above in the rightmost column, for which the objective function is:
$$
\begin{align}
z&=c_{1}x_{1}+c_{2}x_{2}+\dots+c_{n}x_{n}+0\cdot x_{n+1}+0\cdot x_{n+2}+\dots+0\cdot x_{n+m} \\
&=c_{1}\cdot 0+c_{2}\cdot 0+\dots+c_{n}\cdot 0+0\cdot b_{1}+0\cdot b_{2}+\dots+0\cdot b_{m} \\
&=0
\end{align}
$$
In a tableau, **basic variables** have the following property:
- Have a **nonzero coefficient** in only **one row**, and that coefficient is $1$, ie. the column containing the $1$ coefficient otherwise has only $0$s, thus the row placement of the $1$ identifies the $x_{n+j}=b_{j}$ relation

**Optimality Criterion:** if the **objective row** of a tableau has zero entries in the columns labeled by basic variables and **no negative entries** in the columns labeled by **nonbasic variables**, then the solution represented by the tableau is optimal
- Intuitively this is because we start with all nonbasic (non-slack) variables at values of $0$, but the basic variables (slack variables in this case) have coefficients of $0$; we can increase some nonbasic variables, decrease basic variables, and still remain feasible while increasing the objective
- From the definition of adjacent extreme point, it is clear that when we increase a single variable from zero to a positive value we must decrease another variable with a positive value to zero

**Entering Variable:** the nonbasic variable to be increased is called the entering variable, since in the next iteration it will become a basic variable; it will enter the set of basic variables
- The largest increase in $z$ per unit increase in a variable occurs for the **most negative coefficient in the objective row**; choose the nonbasic variable in that respective column
- If there are several possible entering variables (when the most negative entry in the objective row occurs in more than one column), choose any of them
- If the feasible set is bounded, there is a limit on the amount by which we can increase a variable
	- Because of this limit, it may turn out that a larger increase in $z$ may be achieved by **not increasing the variable with the most negative entry** in the objective row; however, we often ignore this and follow the rule above because of its **computational simplicity**
- **Pivotal Column:** the column of the entering variable

**Departing Variable:** the basic variable to be decrease to $0$ is called the departing variable, since in the next iteration it will become a nonbasic variable
- **Pivotal Row:** the row that is labeled with the departing variable
- To ensure we remain feasible, we can express the basic (slack) variables in terms of the nonbasic ones (with all nonbasic variables other than the chosen one set to $0$)
	- Together with the $\geq 0$ constraint on the chosen nonbasic variable, we can put upper bounds on the nonbasic variables relative to the slack variables, and choose the lesser upper bound (the first juncture at which either of the slack variables would become negative and thus not feasible)
	- This is facilitated easily by the tableau; $\theta$-**ratios** are the values in the rightmost column divided by the coefficients in the pivotal column in the respective rows
		- Choose the **smallest positive ratio** as the pivotal row (the pivotal column and pivotal row then together determine the **pivot**)
		- If all ratios are nonpositive, then there is **no finite optimal solution** so the Simplex ceases

**Pivoting:** each iteration of updating the tableau is similar to matrix row reduction
1. Locate the pivot coefficient at the intersection of the pivotal row and pivotal column
2. If the pivot is $k$, multiply the pivotal row by $\frac{1}{k}$ , making the **coefficient in the pivot position** equal to $1$
3. Add suitable multiples of the new pivotal row to all other rows (including the objective row!), so that **all other coefficients in the pivotal column** become $0$
4. In the updated tableau, replace the label on the pivotal row by the entering variable
Once there are **no negative coefficients remaining** in the objective function, the resulting basic variables indicate the optimal BFS
- Any variables that were never pivotal columns remain nonbasic variables (values remain $0$)
- Recall if at any iteration we first encounter all $\theta$-ratios being nonpositive, the method stops early








==For n variables and m independent equality constraints,
Dimension=n−m
provided the constraints are consistent and the nonnegativity constraints do not reduce the dimension further.
So, one equality in three variables gives a plane; two give a line; and three independent equalities give a point (before accounting for nonnegativity restrictions).==