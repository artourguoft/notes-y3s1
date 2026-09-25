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
# <u>Priority Queues and Binary Heaps</u>
Recall:
- **Abstract Data Type (ADT):** theoretical model of an entity and a set of operations that can be performed on that entity
- **Data Structure:** a value in a program which can be used to store and operate on data; an implementation of an ADT

**Priority Queue:** similar to a regular queue ADT, except dequeues are based on the concept of priority rather than simple FIFO, with operations:
- `insert(pq, x, priority)`: adds an element to the priority queue with the given priority value
- `find_max(pq)`: returns the item from the priority queue with the highest priority
- `extract_max(pq)`: same as above, except the element is also removed from the priority queue

A naive unordered linked list implementation of a priority queue can add items in constant time (simply append to front), but search and extraction would scale linearly with the size of the queue; we can improve this by using trees
- **Complete Binary Tree:** a binary tree is complete if:
	1. All of its levels are **full** (ie. have $2^d$ nodes, where $d$ is depth starting at $0$), **except** possibly the bottom one (every leaf must be in one of the two bottommost levels, so at any level there are $\leq 2^d$ nodes)
	2. All of the nodes in the bottom level are as far to the **left** as possible
	- These properties mean that there is only **one complete tree shape** for each **number of nodes**, and the tree is the **smallest possible height** for an $n$ node binary tree (that height being $\lfloor \log_{2}n \rfloor$; note height is defined as **number of edges** from top to bottom - not number of levels)
	- These properties allow CBTs to be represented as arrays with the elements in **level order**; from top to bottom and left to right respectively
- **Max-Heap Property:** the value of each node is greater than or equal to the values of its **children** (and thereby all of its **descendants**)
	- This property is actually less stringent than the binary search tree property; given a tree which satisfies the max-heap property, we **cannot conclude anything** about the relationship between its **left and right subtrees** (ex. nodes in left can be greater than nodes in right)
- **Max-Heap:** a CBT that satisfies the max-heap property, and if **indexed starting at** $1$ in an array $H$, has the following properties:
	- Left child of $H[i]$ is at $H[2i]$
	- Right child of $H[i]$ is at $H[2i+1]$
	- Parent of $H[i]$ is at $H\left[ \left\lfloor  \frac{i}{2}  \right\rfloor \right]$ (given $i>1$, since the root has no parent)
- **Min-Heap Property:** the value of each node is less than or equal to the values of its children (and thereby all of its descendants)
- **Min-Heap:** a CBT that satisfies the min-heap property; minimum key is the root, etc. otherwise the same as a max-heap

For the max-heap to be a correct implementation of a priority queue, we implement the necessary operations while maintaining completeness and max-heap properties:
- `insert(H, x)`: $\Theta(\log_{2}n)$
	- To insert, start by appending the new element to the end of the array $H[\text{size }+1]$
	- This slot represents the leftmost empty slot at the leaf levels of the tree, so appending there maintains completeness, and is $\Theta(1)$
	- Then, recursively swap the new element with its parent at $H\left[ \left\lfloor  \frac{i}{2}  \right\rfloor \right]$ until the max-heap property is restored
	- Intuitively, this operation is $\Theta(\log_{2}n)$ since the new element at most has to be swapped from the bottom all the way to the root, traversing the whole height of the tree (which is $\lfloor \log_{2}n \rfloor$)
- `find_max(H)`: $\Theta(1)$
	- Facilitated by the max-heap property, this simply returns the first element of $H$ (the root of the tree), and thus is $\Theta(1)$
- `extract_max(H)`: $\Theta(\log_{2}n)$
	- This returns and removes the first element of $H$, which is $\Theta(1)$
	- With the root removed, swap the final element into the root $H[1]=H[\text{size}]$; this maintains completeness (and can be done in $\Theta(1)$ since we know the size of the heap)
	- Then, recursively compare the priority of the moved node with both its children and swap with the **higher priority** child **if higher priority than the parent**, until both children of the moved node are such that the max-heap property is restored, or until the node is once again a leaf
		- `max_heapify(H, i)`: the recursive algorithm that implements the above specification for a max-heap index `i`, with **precondition** that the left and right children of node `i` are max-heaps (satisfied since we are working with an existing max-heap)
			- By the Master Theorem this algorithm is $O(\log_{2}n)$ since each recurrence is constant time and the number of recurrences is at most the height of the tree (which is $\lfloor \log_{2}n \rfloor$) since the new element at most has to be swapped from the top to the leaf level

**Heapsort:** given a heap, we can get a sorted list of the elements of the heap by simply repeatedly calling `extract_max()`
- Like mergesort, but unlike insertion sort, heapsort’s running time is $O(n\log_{2}n)$ as we will show shortly; like insertion sort, but unlike mergesort, heapsort sorts in place; **best of both worlds!**
- Heapsort assumes we have a heap; if we first receive an **unordered** array $A$ of inputs (representing a **CBT that does not yet satisfy the max-heap property**), we must first turn the array into a max-heap via a `build_max_heap()` algorithm:
	- Looping for $i\in[\lfloor \frac{n}{2} \rfloor:1]$, repeatedly call `max_heapify(A, i)`
		- Note that $A[\lfloor \frac{n}{2} \rfloor+1 : n]$ represents the elements of the **leaf level** of a CBT, which are already valid max-heaps (they have only one element!); so we only need to go through the levels one up from that up to the root (so we loop over the backward range above)
			- This also ensures that the precondition of both subtrees being max-heaps is satisfied, since $A$ is not a valid max-heap at first here 
		- Intuitively, `build_max_heap()` is $O(n\log_{2}n)$ since it calls `max_heapify(A, i)` $n$ times; however we can show a **linear** $\Theta(n)$
			- First, note that `max_heapify(A, i)` is $O(\log_{2}n)$ where $\lfloor \log_{2}n \rfloor$ is the height of the max-heap $A$; thus we can rewrite this as $O(h)$ where $h$ is the height of the max-heap in context
			- Then consider that $h$ is **not static as we move up the levels** of the CBT; for a given index $i$ the height $h$ is the largest height of this $i^{\text{th}}$ tree's subtrees (increasing as we go up; and recall height is number of edges to leaf)
			- The trivial and loose lower bound is $\Omega(n)$ in the specific case of $A$ already happening to be a max-heap; then we simply iterate over $\left\lfloor  \frac{n}{2}  \right\rfloor$ nodes at each of which `max_heapify(A, i)` runs in constant time and does not recurse
			- For the upper bound, recall CBTs have $\leq 2^d$ nodes at depth $d$, and the height of any subtree at that depth is $\leq \lfloor \log_{2}n \rfloor-d$  (so at each depth `max_heapify()` is $\leq O(\lfloor \log_{2}n \rfloor-d)$); then build algorithm cost is:
$$
\begin{align}
&\leq \sum_{d=0}^{\lfloor \log_{2}n \rfloor -1}2^d \cdot (\lfloor \log_{2}n \rfloor -d) \\
& = \sum_{i=1}^{\lfloor \log_{2}n \rfloor} 2^{\lfloor \log_{2}n \rfloor -i}\cdot i \quad\text{ where }i=\lfloor \log_{2}n \rfloor - d \\
& = 2^{\lfloor \log_{2}n \rfloor -1} \cdot \sum_{i=1}^{\lfloor \log_{2}n \rfloor} \frac{i}{2^i} \\
& \leq n \cdot \sum_{i=1}^{\lfloor \log_{2}n \rfloor} \frac{i}{2^i} \\
& \leq n \cdot \sum_{i=1}^\infty \frac{i}{2^i} \\
& = 2n
\end{align}
$$
	- Notice this is all **in-place**; the input array is assumed and is interpreted as a CBT - this algorithm **does not fill an empty array**; in those cases we `insert()` $n$ times for an overall $O(n\log_{2}n)$
- Once we have a max-heap, we call `extract_max(H)` $n$ times each of which is $\Theta(\log_{2}n)$ for a **total result** of $\Theta(n\log_{2}n)$ for building a max-heap and then heapsorting from it
# <u>Binomial Heaps</u>
The preceding **binary heap** implementation of the priority queue does not allow for an efficient **union** operation, wherein we would merge two heaps into one new valid heap (would have to simply merge the two arrays and then run the `build_heap()` algorithm, which recall was $\Theta(n)$)
- Binomial heaps allow us to perform unions in $O(\log_{2}n)$ at the expense of taking simple retrievals from $O(1)$ to $O(\log_{2}n)$ compared to binary








BST deletes, node to be deleted $z$:
- When $z$ has $0$ children; simply delete the node and set the relevant pointer in its parent to null; this is $\Theta(1)$
- When $z$ has $1$ child (regardless of left or right); delete the node and set the parent of $z$ to now point directly to the child of $z$; this is $\Theta(1)$
- When $z$ has $2$ children; find the successor of $z$ (smallest key $>z$); this is done by going to the right child first, then repeatedly going to the left child until leaf 
	- Swap the successor into what was $z$ (this is still a valid BST at this point, but most use cases will not want duplicate data); then do delete algorithm (with $0\vee1$ child case, since the successor by definition has no left child) on successor; this is $\Theta(h)$ where $h=n$ for worst tree

AVL deletes, node to be deleted $z$:
- Similar to BST delete but with extra logic for rebalancing; deletes in AVL always result in removing a leaf
- When $z$ has $0$ children; simply delete the node and set the relevant pointer in its parent to null; this is $\Theta(1)$
- When $z$ has $1$ child (regardless of left or right); that child is necessarily a leaf (if it wasn't the AVL property would be violated since this subtree would then be higher than $1$ while the other is $0$)
	- Thus, simply delete the node and set the relevant pointer in its parent to null; this is $\Theta(1)$
		- This will move the parent's balance from $\pm 1$ (depending on left or right child) to $0$, maintaining the AVL property
- When $z$ has $2$ children; find the successor of $z$ (smallest key $>z$); this is done by going to the right child first, then repeatedly going to the left child until leaf 
	- Swap the successor into what was $z$; then do delete algorithm (with $0\vee1$ child case, since the successor by definition has no left child) on successor; this is $\Theta(\log_{2}n)$ since AVLs are balanced (compared to $\Theta(n)$ for BST)
The difference from BSTs hereon is that balance factors have to be updated and rebalancing rotations may be needed after deletion:
- These algorithms are the same as in inserts; ie. update balance factors up from deleted node and rotate at first imbalance