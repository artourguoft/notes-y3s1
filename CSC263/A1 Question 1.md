First we determine $T(n)$:
- **Line 1:** `n = A.size` is a one step / constant time assignment regardless of the size of the input
- **Line 2:** `for i = 1 to n` outer loop runs at most $n$ times by definition; worst-case is that the conditions on Line 4 and Line 5 are never met
- **Line 3:** `for j = 1 to n` inner loop runs at most $n$ times by definition, per each iteration of the outer loop; worst-case is that the condition on Line 5 is never met
- **Line 4:** `if ... return` arithmetic and comparison in condition check is one step / constant time; worst-case is that the condition is never met, which gives $n\cdot n=n^2$ one step checks of the condition; $n$ iterations from the inner loop for each of $n$ iterations of the outer loop
- **Line 5:** `if ... return` arithmetic and comparison in condition check is one step / constant time; worst-case is that the condition is never met, which gives us $n$ one step checks of the condition; once for each of $n$ iterations of the outer loop

Summing up, we get we a worst-case runtime of $T(n)=n^2+n+1$


**WTP:** $T(n)\in O(n^2)$ meaning $\exists c\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies n^2+n+1\leq c\cdot n^2$
- Take $c=3\in \mathbb{R}$
- Take $n_{0}= 2\in \mathbb{N}$, satisfying assumption that input array has $\geq 2$ elements
- Let $n\in \mathbb{N}$ 
- Assume $n\geq n_{0}=2$, from which:
$$
\begin{align}
n^2+n+1&< n^2+n^2+1 \\
&<n^2+n^2+n\\
&<n^2+n^2+n^2 \\
&=3n^2 \\
&=c\cdot n^2
\end{align}
$$

**WTP:** $T(n)\in \Omega(n^2)$ meaning $\exists c\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies n^2+n+1\geq c\cdot n^2$
- Take $c=1\in \mathbb{R}$
- Take $n_{0}= 2\in \mathbb{N}$, satisfying assumption that input array has $\geq 2$ elements
- Let $n\in \mathbb{N}$ 
- Assume $n\geq n_{0}=2$, from which:
$$
\begin{align}
n^2+n+1&> n^2 =c\cdot n^2\\
\end{align}
$$

Since $T(n)\in O(n^2)$ and $T(n)\in \Omega(n^2)$, by definition $T(n)\in \Theta(n^2)$
