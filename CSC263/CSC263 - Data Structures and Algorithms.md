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

Recall:
- **Abstract Data Type (ADT):** theoretical model of an entity and a set of operations that can be performed on that entity
- **Data Structure:** a value in a program which can be used to store and operate on data; an implementation of an ADT
# <u>Binary Heaps</u>
**Priority Queue:** similar to a regular queue ADT, except dequeues are based on the concept of priority rather than simple FIFO, with operations:
- `insert(pq, x, priority)`: adds an element to the priority queue with the given priority value
- `find_max(pq)`: returns the item from the priority queue with the highest priority
- `extract_max(pq)`: same as above, except the element is also removed from the priority queue

A naive unordered linked list implementation of a priority queue can add items in constant time (simply append to front), but search and extraction would scale linearly with the size of the queue; we can improve this by using trees
- **Complete Binary Tree:** a binary tree is complete if:
	1. All of its levels are **full** (ie. have $2^d$ nodes, where $d$ is depth starting at $0$), **except** possibly the bottom one (every leaf must be in one of the two bottommost levels, so at any level there are $\leq 2^d$ nodes)
	2. All of the nodes in the bottom level are as far to the **left** as possible
	- These properties mean that there is only **a unique complete tree shape** for each **number of nodes**, and the tree is the **smallest possible height** for an $n$ node binary tree (that height being $\lfloor \log_{2}n \rfloor$; note height is defined as **number of edges** from top to bottom - not number of levels)
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
- **Binomial heaps** allow us to perform unions in $O(\log_{2}n)$ at the expense of taking `find_extreme()` operations from $O(1)$ to $O(\log_{2}n)$, and the ADT becomes known as a **mergeable priority queue** 

**Binomial Tree:** defined recursively where $B_{0}$ is a single node, and for $k\geq 1$ then $B_{k}$ is the tree where the root has $k$ children (not total descendants!), each of which are the binomial trees $B_{0},B_{1},\dots,B_{k-1}$
- The **height** of a $B_{k}$ binomial tree is $k$
- The **number of nodes** at depth $d$ is $\binom{k}{d}$, the **total number of nodes** is $2^k$, and the **number of edges** is $2^k-1$
- To create $B_{k}$ we take two $B_{k-1}$ trees, and attach as a child of the other's root
	- Two min-heap ordered $B_{k}$ trees can be merged into a single $B_{k+1}$ tree (maintaining min-heap order) with **one key comparison**; simply check which one's root is lesser and attach it as a child to the other's root
		- We cannot merge two binary heaps so easily!
**Binomial Forest:** a binomial forest $F_{n}$ is a sequence of $B_{k}$ trees with strictly decreasing $k$ and a total of $n$ nodes
- If we represent $n$ as a binary number, the high bits indicate which $B_{k}$ trees should be included in the forest, ex. $n=7=(111)_{2}$ implying that $F_{7}$ contains $B_{2},B_{1},B_{0}$
	- This follows from the size of each $B_{k}$ being $2^k$
	- Recall in general that for $n=(b_{t},b_{t-1},\dots,b_{0})_{2}$ we have $t=\lfloor \log_{2}n \rfloor$ and at most $\lfloor \log_{2}n \rfloor + 1$ high bits
	- Let $\alpha(n)$ be the **number of high bits** in the binary, then $F_{n}$:
		- Is made of $a(n)$ binomial trees
		- Has $n-\alpha(n)$ edges; intuitively since there is no ultimate root node connected to each $B_{k}$
- **Deleting the root** of a binomial tree $B_{k}$ **yields the binomial forest** $F_{n}$ such that $n=(1,1,\dots,1)_{2}$ where there are $k$ ones

A **min binomial heap** stores its elements in a binomial forest such that each contained $B_{k}$ satisfies the **min-heap property**
 - A binomial heap for $n$ elements can be built with $O(n)$ key comparisons (from an unsorted array), leading to $n$ edges
 - The **three pointers** necessary in each node are a **parent** pointer, **left child** pointer and **right sibling** pointer
	 - The roots of each $B_{k}$ tree are connected with the sibling pointer to the other trees, but going leftward instead with the head pointer pointing to the lowest order tree's root
- **Deleting the root** of a min-heap ordered binomial tree $B_{k}$ **yields a min binomial heap** of all $B_{0},\dots,B_{k-1}$ trees
- `union(H, G)`: $O(\log_{2}n)$
	- For min binomial heaps $H,G$, perform addition of their binary representations; the resulting binary yields the union min binomial heap
		- The number of **added edges** is the **number of key comparisons**, which is represented by **carried ones** during binary addition (since those represent $k$ values where both heaps already had trees and thus needed to be merged, which recall requires just comparing their roots)
	- Assuming $|H|,|G|<n$, then recall both have $\leq\lfloor \log_{2}n \rfloor + 1$ high bits representing $B_{k}$ trees, and thus the union will do at most that many comparisons (in the case of both binomial heaps containing all the same binomial trees)
- `insert(H, {x})`: $\Theta(\log_{2}n)$
	- Performed as a `union()` with a singleton set of the element to insert
	- Note, for $k>\log_{2}n$ successive inserts the total cost is $\leq2k$ key comparisons, meaning the **average cost per insert** is $\leq 2$ comparisons 
- `find_min(H)`: $\Theta(\log_{2}n)$, worse than $\Theta(1)$ in binary heaps because there is no relationship between neighbouring binomial trees in the binomial heap, so at worst will have to check the roots of all the contained trees of which there are $\leq\lfloor \log_{2}n \rfloor + 1$
- `extract_min(H)`: $\Theta(\log_{2}n)$
	- Performed by first doing `find_min()` and deleting that node, which recall leaves us with a min binomial heap, that is now disconnected from the whole original binomial heap; then `union()` this resulting binomial heap with the remainder of the original one
# <u>Binary Search Trees</u>
**Dictionary:** set $S$ of elements with keys, with operations:
- `search(S, x)`: returns the element in $S$ with key $x$, or error
- `insert(S, x)`: inserts $x$ into $S$
- `delete(S, x)`: deletes the element in $S$ with key $x$, or error

The simplest dictionary implementation is the **linked list**:
- `search(S, x)`: $\Theta(n)$
- `insert(S, x)`: $\Theta(1)$
- `delete(S, x)`: $\Theta(n)$

The **binary search tree** allows us to perform **all** of these operations in $\Theta(h)$ where $h$ is **height**:
- The **BST property**; each node's left subtree has only keys $\leq$ its key, and right subtree only keys $\geq$
- When stored in an array, BSTs are walked through via **in-order traversal** which leads to the elements being stored in **ascending order**; traverse the left subtree recursively, then the node, then the right subtree
	- BST shapes are **not unique** for the array of ascending order keys they represent; they can be rotated around the root, for example 
- `search(S, x)`: $\Theta(h)$
	- Simply traverse the tree from the root, comparing the key at each node until the key or a leaf is found
- `insert(S, x)`: $\Theta(h)$
	- Simply traverse the tree from the root, comparing the key at each node then append where appropriate
	- Note, BST inserts will **always append a leaf**
- `delete(S, x)`: $\Theta(h)$
	- If the node to be deleted **is a leaf** (has no children), simply remove it in $\Theta(1)$
	- If the node to be deleted **has one child**, replace it with that subtree in $\Theta(1)$
	- If the node to be deleted **has both children**
		- Find the successor or predecessor and replace the node with it; then replace the successor or predecessor with its child if it has one (will have **at most one** by its definition)
			- The **successor** is the next node in the array; farthest left node of the right subtree, which then by definition will have at most one child
			- The **predecessor** is the previous node in the array; farthest right node of the left subtree, which then by definition will have at most one child
			- Note, both of the traversals above will be worst case $\Theta(h)$
All of these operations are $\Theta(h)$, but consider that an ascending order array can be put into a **single-line** BST where $h=n-1$, making all these operations **linear**
- There are multiple enhanced forms of BSTs, we will focus on AVL trees
# <u>AVL Trees</u>
AVL trees seek to have balanced subtree heights at every node in the tree:
- **Balance Factor:** at any node $v$, we have $BF(v)=h_{r}-h_{l}$
	- For an AVL tree $T$, for every node $v\in T$ we require $-1\leq BF(v)\leq 1$
	- If $BF(v)\leq -2$ it is left heavy, if $BF(v)\geq 2$ it is right heavy

AVL trees of $n$ nodes have height $\leq 1.44\log_{2}(n+2)$, so $h\in \Theta(\log_{2}n)$ and thus can do `insert()` and `delete()` in $\Theta(\log_{2}n)$ all while maintaining necessary balance factor
- `search(T, x)`: $\Theta(\log_{2}n)$
	- Same as regular BST search
- `insert(T, x)`: $\Theta(\log_{2}n)$
	- Insert same as in regular BST, then traverse up to the root and update BFs; on the first imbalanced node encountered perform **left and / or right rotations** until balanced; all in $\Theta(1)$ (just changing pointers)
		- If $BF(v)=2$ and $BF(v.right)=1$, perform a **left** rotation about $v$
		- If $BF(v)=2$ and $BF(v.right)=-1$, perform a **right** rotation about $v.right$, then a **left** rotation about $v$
		- If $BF(v)=-2$ and $BF(v.left)=-1$, perform a **right** rotation about $v$
		- If $BF(v)=-2$ and $BF(v.left)=1$, perform a **left** rotation about $v.left$, then a **right** rotation about $v$
	- After rebalancing the **first** encountered imbalanced subtree, its **height** is restored to what it was **before the insertion**, therefore no further ancestors can become imbalanced and we stop traversing upward to update balance factors
- `delete(T, x)`: $\Theta(\log_{2}n)$
	- When $x$ has $0$ children; simply delete the node and set the relevant pointer in its parent to null in $\Theta(1)$
	- When $x$ has $1$ child (regardless of left or right); that child is necessarily a leaf (if it wasn't the AVL property would be violated since this subtree would then be higher than $1$ while the other is $0$)
		- Thus, simply delete the node and set the relevant pointer in its parent to null; this is $\Theta(1)$
		- This will move the parent's balance from $\pm 1$ (depending on left or right child) to $0$, maintaining the AVL property
		- When $x$ has $2$ children; do the BST predecessor / successor algorithm; this is worst-case $\Theta(\log_{2}n)$ since AVLs are balanced (compared to worst-case $\Theta(n)$ for BST)
	- The difference from BSTs is that balance factors have to be updated and rebalancing rotations may be needed after deletion
		- Note the same fact about preserving heights after insertion is **not true for deletion**; we may need to update balance factors all the way up to the root regardless of where the imbalance initially was after deletion  
# <u>Augmentation</u>
Designing augmented data structures for **additional operations**:
1. Choose the **base data structure** given the runtime class requirements and types of operations
2. Choose what to use as the **key** values and what **additional data** should be stored to facilitate the additional operations
3. Ensure the augmentations above can be **updated cheaply** (constant time or within required runtime classes) during and after **all operations** (not necessarily when building the initial structure)
4. Prove how the supported operations work and their runtime
# <u>Hash Tables</u>
If we simply store dictionaries in direct-access tables like arrays **indexed and ordered by keys**, then all operations become $\Theta(1)$ since we can simply perform pointer arithmetic with key values; ex. store counts of each ASCII code (used as the key, with 128 possible keys) in a plaintext file
- This does not work when the universe of keys $|U|$ becomes large and infeasible to store (in memory or otherwise)

However, if we can store a table $T$ with $m<|U|$ keys we can use **hash tables**
- Consider a **hash function** $h:U\to \{ 0,1,\dots,m-1 \}$, then for any key $k\in U,h(k)=i$ where $i\in\{ 0,1,\dots,m-1 \}$ we say $k$ **hashes to** $i$
- Assume that the has function **always hashes in** $\Theta(1)$ for any valid key input
	- But since $m<|U|$, by the Pigeonhole Principle th]ere must be pairs of keys that have **hash collisions**, ie. different keys hashing to the same value
		- We resolve this by **chaining**; store a **doubly linked list** at every slot, then:
- `insert(x)`: $\Theta(1)$
	- Inserting into slot a given $T[h(key(x))]$ appends to the head of the list at this slot
- `delete(x)`: $\Theta(1)$
	- Given that we have a direct pointer to $x$ 
- `search(k)`: $\Theta(n)$
	- Searching the chain at $T[h(k)]$ depends on the **length of the chain**, and in the worst case **all** $n$ **keys hash to the same** $m$!

The above are **worst-case** runtimes, but with hashing we want to consider the length of the chains on average; given we are inserting $n$ keys into an initially empty table $T$ of size $m$:
1. Assume SUHA; **simplified uniform hashing assumption**
	- Each key $k$ that we insert is **uniformly likely** to hash to any value in $\{ 0,1,\dots,m-1 \}$, ie. $P(h(k)=i)=\frac{1}{m}$ for $0\leq i\leq m-1$
	- Under SUHA, the **expected length** of any chain $E(|T[i]|)=\frac{n}{m}=\alpha$ where $\frac{n}{m}=\alpha$ is called the **load factor**
	- Then, a **successful search** (assuming each stored key is equally likely to be searched) will traverse on average half of the elements and thus has an **expected runtime** $\Theta( \frac{\alpha}{2})$, and an **unsuccessful search** will traverse all elements and thus has an **expected runtime** $\Theta\left( \frac{n}{m} \right)=\Theta(\alpha)$
2. Assume $\alpha$ is bounded by a constant by ensuring that $m\in\Theta(n)$
	- If we keep $m$ constant, then as $n$ grows the load factor grows (chains get longer), so search runtime scales linearly with $n$
	- Thus, we ensure $m$ scales linearly with $n$ (meaning $\frac{n}{m}\leq c \iff n\leq cm$) which makes $\alpha$ a bound by the **constant** $\alpha\leq c$, and thus **any search becomes** expected $\Theta(1)$
		- Practically, if $n$ grows and we get $n>cm$ then we reallocate a larger array with a larger size $m$ and rehash to it, to maintain the necessary bounds




- **Bloom filters**:
	- Array of size $m$ bits, with $t$ **independent** hash functions $h_{0},h_{1},\dots,h_{t}$, each of which is $h_{i}:U\to \{ 0,1,\dots,m-1 \}$, satisfies SUHA, and hashes in $\Theta(1)$
	- Then insertions hash the input with **each** function and set (or keep) each resultant bit at high
	- And searches hash the input with **each** function and check that **all** resultant bits are set to high
	- Probability of false positive, by independence and SUHA etc, based on probability of any given bit being positive after $n$ inserts being $1-\left( 1-\frac{1}{m} \right)^{nt}=1-e^{-nt/m}$ is $(1-e^{-nt/m})^t$
		- Then set the derivative of this to $0$ and solve for $t$ which yields $t=0.69\frac{m}{n}$ for lowest false positive rate (where $m$ is bits per element)

- **Randomized quicksort**:
	- Two keys are compared only if one of them is selected as a pivot
	- Two keys are compared at most once
	- If two keys are split apart into different sets, they are never compared
	- Ultimately the expected number of comparisons is $O(n\log_{2}n)$
		- Namely, $E(C)=\sum_{1\leq i\leq j\leq n}\frac{2}{j-i+1}$

- Starting with size $n$ **disjoint singleton set forest**, sequence $\sigma$ of $n-1$ unions with $m\geq n$ finds (finding representative, worst case traversing entire height):
	- All runtimes have the find complexity absorb the $n-1$ unions because $m\geq n$
	- Simplest case is doing all unions first but creating a linear tree, so $\Omega(mn)$ for $m$ finds after that and each find traversing all nodes to root
	- With **WU** is $O(m\log_{2}n)$
		- By induction, every tree $T$ of height $h$ created by WU has $n\geq|T|\geq 2^h$, from which $\log_{2}n\geq h$ 
	- With **WU** and **PC** is $O(m\log_{2}^*n)$
		- Where $\log_{2}^*n=\min\{k:2^{*k}\geq n \}$ 
		- In fact, there is a lower bound $\Omega(m \alpha^{-1}(m,n))$ where $\alpha^{-1}$ is essentially a constant function, so $\Omega(m)$














































Interval trees:
- Overlap of two intervals $i,i'$ happens when $lo(i)\leq hi(i')\wedge lo(i')\leq hi(i)$
- Goal: store a set of intervals, support all 3 dictionary operations (search specifically will just return any overlapping interval) and all in $O(\log_{2}n)$ where $n$ is the number of intervals
- We choose AVL as the base data structure since we need a dictionary with ordering and $O(\log_{2}n)$ operations
- We choose the key as $lo(i)$ in each node for each interval $i$
- We choose to store $hi(i)$ and $\max(i)$ where the max is the largest $hi$ in the $i$ subtree, in each node for each interval $i$
	- Then updating these is constant time (simply max operation on both subtrees' maxes and the nodes hi, plus any possible rotations) and at most you will have to go thru $\log_{2}n$ nodes from root to leaf doing this
- Then for search, starting at the root
```
search(x)
	y = root
	while y != null && x and y do not overlap
		if y.left = null || max(y.left) < low(x)
			y = y.right
		else
			y = y.left
	return y
```
- When the root is an overlapping interval, we return it
- Otherwise, the loop takes us closer to an overlapping interval
	- Loop Invariant: if there is an overlapping interval, there will be one such interval in y subtrees
		- Base case trivial for root
		- After a loop iteration
			- If y.left is null then the answer must be in y.right
			- If max(y.left) < lo(x) then no interval in the left subtree intersects with x, so again answer must be in y.right
			- If max(y.left) >= lo(x) then let I be the interval with hi = that max
				- If I intersects with x then we're fine, if I is entirely greater than x then once we get to it 

A3Q5 Example (words from a dictionary of size $l$)
- What data structures to use?
	- Hash table $T$ of size $m$ using chaining, where $m\geq l$
	- Direct access array $A$ initially all null, accessed with each letter from a-z
- What to store in each?
	- Store each word and its count 
	- `A[c]` is most frequent word starting with c
- What assumptions are we making?
	- SUHA, needed to achieve constant expected time for insert and query
	- Hashing a word takes $\Theta(1)$ time
- Explain in English how the algorithm works
	- Whenever a word comes in, we has it to map it to a chain in table $T$
	- If the word is in the chain, increment its count; otherwise store it with a count of 1
	- Then check corresponding entry in A (hash table lookup) to see if the new word has higher count; if yes replace, if no do nothing
	- For queries, output `A[a],...,A[z]`
- Pseudocode:
```
Insert(w)
	h = hash(w)
	if W is in the chain T[h]
		increment counter
	else
		store with count of 1 in chain T[h]
	if T[w] > T[A[w[0]]] or (T[w] > T[A[w[0]]] and w < A[w[0]]) # second part checks
		A[w[0]] = w                                             # 'dictionary order'

Query()
	for i in [a,z]
		output A[i]
```
- Runtime:
	- Query: at most 26 iterations of constant operations, so $O(1)$
	- Insert: SUHA and $m\in\Theta(l)$ means length of chain is $\Theta(1)$ n expectation and searching chain is $\Theta(1)$ in expectation, essentially accessing a hash table is $\Theta(1)$ expected