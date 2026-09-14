# <u>Running Times</u>
Let $t(x)$ be the number of steps or runtime taken to complete by algorithm $\mathcal{A}$ on input $x$, and let $T(n):\mathbb{N}\to \mathbb{R}_{+}$ be the **worst-case** time complexity of algorithm $\mathcal{A}$ on inputs of size $n\in \mathbb{N}$:
$$
T(n)=\max(\{ t(x_{n}):x_{n}\text{ is an input of size }n \})
$$
Then the definitions of the bounds on this worst-case runtime are:
- $O(f(n)): \exists c\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies T(n)\leq c\cdot f(n)$ where the consequent is equivalent to $\forall x_{n},t(x_{n})\leq c \cdot f(n)$
- $\Omega(f(n)): \exists c\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies T(n)\geq c\cdot f(n)$ where the consequent is equivalent to $\exists x_{n},t(x_{n})\geq c \cdot f(n)$
- $\Theta(f(n)): \exists c_{1},c_{2}\in \mathbb{R}_{+},n_{0}\in \mathbb{N},\forall n\in \mathbb{N},n\geq n_{0}\implies c_{1}\cdot f(n)\leq T(n)\leq c_{2}\cdot f(n)$

Given the formal definitions above:
- $T(n)\in O(f(n))$ means that $T$ is eventually bounded above by a constant factor of $f$; eventually **every** input of each size $n$ takes at most $c\cdot f(n)$
- $T(n)\in \Omega(f(n))$ means that $T$ is eventually bounded below by a constant factor of $f$; eventually **some** input of each size $n$ takes at least $c\cdot f(n)$
- $T(n)\in \Theta(f(n))$ means that $f$ and $T$ eventually grow at the same rate and thus have only a constant factor difference
- Note the slight difference in the consequents of $O$ and $\Omega$; since $T$ is a maximum, proving an upper bound requires a universal proof over the set of runtimes, whereas proving a lower bound requires only an existential proof (since any runtime is less than or equal to the maximum runtime)
- Also note that $O$ and $\Omega$ alone are **not necessarily tight** bounds; for $O$ we can just pick any function that grows faster than $T$, and for $\Omega$ we could even take the minimum runtime for each $n$ as a very loose and uninformative lower bound
	- This is the value of finding an $\Theta$ class; this by definition is a tight upper and lower bound and thus reflects the exact growth rate of the worst-case runtime function, differing only by a constant factor!
# <u>Priority Queues and Heaps</u>
Recall:
- **Abstract Data Type (ADT):** theoretical model of an entity and a set of operations that can be performed on that entity
- **Data Structure:** a value in a program which can be used to store and operate on data; an implementation of an ADT

**Priority Queue:** similar to a regular queue ADT, except dequeues are based on the concept of priority rather than simple FIFO, with operations:
- `insert(pq, x, priority)`: adds an element to the priority queue with the given priority value
- `find_max(pq)`: returns the item from the priority queue with the highest priority
- `extract_max(pq)`: same as above, except the element is also removed from the priority queue

A naive unordered linked list implementation can add items in constant time (simply append to front), but search and extraction would scale linearly with the size of the queue; we can improve this by using trees
- **Complete Binary Tree:** a binary tree is complete if:
	1. All of its levels are **full** (ie. have $2^{\text{level}}$ nodes), **except** possibly the bottom one (every leaf must be in one of the two bottommost levels)
	2. All of the nodes in the bottom level are as far to the **left** as possible
	- These properties mean that there is only **one complete tree shape** for each **number of nodes**, and the tree is **smallest possible height** for $n$ nodes (that height being $\lfloor \log_{2}n \rfloor$; note height is defined as **number of edges** from top to bottom - not number of levels)
	- These properties allow CBTs to be represented in **level** order, from top to bottom and left to right respectively
- **Max-Heap Property:** a tree satisfies the max-heap property if the value of each node is greater than or equal to the values of all its descendants
	- This property is actually less stringent than the binary search tree property; given a node which satisfies the max-heap property, we cannot conclude anything about the relationships between its left and right subtrees
- **Max-Heap:** a CBT that satisfies the max-heap property, and if **indexed starting at** $1$ in an array $H$ has the following properties:
	- Left child of $H[i]$ is at $H[2i]$
	- Right child of $H[i]$ is at $H[2i+1]$
	- Parent of $H[i]$ is at $H\left[ \left\lfloor  \frac{i}{2}  \right\rfloor \right]$, given $i>1$ (ie. the root has no parent)

For the max-heap to be a correct implementation of a priority queue, we implement the necessary operations while maintaining completeness and max-heap properties:
- `insert(H, x)`: to insert, start by appending the new element to the end of the array $H[\text{size }+1]$
	- This slot represents the leftmost empty slot at the bottom levels of the tree, so appending there maintains completeness (and is $\Theta(1)$)
	- Then, recursively swap the new element with its parent at $H\left[ \left\lfloor  \frac{i}{2}  \right\rfloor \right]$ until the max-heap property is restored
	- Intuitively, this operation is $\Theta(\log_{2}n)$ since the new element at most has to be swapped from the bottom all the way to the root, traversing the whole height of the tree (which is $\lfloor \log_{2}n \rfloor$)
- `find_max(H)`: facilitated by the max-heap property, this simply returns the first element of $H$ (the root of the tree), and thus is $\Theta(1)$
- `extract_max(H)`: this returns but also removes the first element of $H$
	- With the root removed, swap the final element into the root $H[1]=H[\text{size}]$; this maintains completeness (and can be done in constant time since we know the size of the heap)
	- Then, recursively compare the priority of the new root with both its children and swap with the **higher priority** child until both children are such that the max-heap property is restored, or until the element is once again a leaf
	- Again, this operation is $\Theta(\log_{2}n)$ since the new element at most has to be swapped from the top all the way to the leaf level, traversing the whole height of the tree (which is $\lfloor \log_{2}n \rfloor$)

**Heapsort:**