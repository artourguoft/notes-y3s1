# <u>Running Times</u>
Let $t(x)$ be the number of steps or runtime taken to complete by algorithm $\mathcal{A}$ on input $x$, and let $T(n):\mathbb{N}\to \mathbb{R}_{+}$ be the **worst-case** time complexity of algorithm $\mathcal{A}$ on inputs of size $n\in \mathbb{N}$:
$$
T(n)=\text{max}(\{ t(x_{n}):x_{n}\text{ is an input of size }n \})
$$
Then the definitions of the bounds are:
- $O(f(n)): \exists c\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies T(n)\leq c\cdot f(n)$ where the consequent is equivalent to $\forall x_{n},t(x_{n})\leq c \cdot f(n)$
- $\Omega(f(n)): \exists c\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies T(n)\geq c\cdot f(n)$ where the consequent is equivalent to $\exists x_{n},t(x_{n})\geq c \cdot f(n)$
- $\Theta(f(n)): \exists c_{1},c_{2}\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies c_{1}\cdot f(n)\leq T(n)\leq c_{2}\cdot f(n)$

Given the formal definitions above:
- $T(n)\in O(f(n))$ means that $T$ is eventually bounded above by a constant factor of $f$; eventually **every** input of each size $n$ takes at most $c\cdot f(n)$
- $T(n)\in \Omega(f(n))$ means that $T$ is eventually bounded below by a constant factor of $f$; eventually **some** input of each size $n$ takes at least $c\cdot f(n)$
- $T(n)\in \Theta(f(n))$ means that $f$ and $T$ eventually grow at the same rate and thus have only a constant factor difference
- Note the slight difference in the consequents of $O$ and $\Omega$; since $T$ is a maximum, proving an upper bound requires a universal proof over the set of runtimes, whereas proving a lower bound requires only an existential proof (since any runtime is less than or equal to the maximum runtime)
- Also note that $O$ and $\Omega$ alone are **not necessarily tight** bounds; for $O$ we can just pick any function that grows faster than $T$, and for $\Omega$ we could even take the minimum runtime for each $n$ as a very loose and uninformative lower bound
	- This is the value of finding an $\Theta$ class; this by definition is a tight upper and lower bound, differing only by a constant factor!

