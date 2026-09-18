BST deletes, node to be deleted $z$:
- When $z$ has $0$ children; simply delete the node and set the relevant pointer in its parent to null; this is $\Theta(1)$
- When $z$ has $1$ child (regardless of left or right); delete the node and set the parent of $z$ to now point directly to the child of $z$; this is $\Theta(1)$
- When $z$ has $2$ children; find the successor of $z$ (smallest key $>z$); this is done by going to the right child first, then repeatedly going to the left child until leaf 
	- Swap the successor into what was $z$ (this is still a valid BST at this point, but most use cases will not want duplicate data); then do delete algorithm (with $0\vee1$ child case, since the successor by definition has no left child) on successor; this is $\Theta(h)$

Max-heaps:
- Max-heap property implies that any traversal from root to leaf will be non-increasing
- Max-heap property tells us nothing about relation between subtrees; ex. left and right subtrees of the root have no relation to each other'
- Consider `max_heapify(array, index)`
```Python
'''
Preconditions: array A, index i, where left and right subtrees of i are valid max-heaps
Postconditions: subtree at i is a valid max-heap
'''
def max_heapify(A, i)
	# Compare A[i] to both children
	# If A[i] <= largest child, swap with it
    # If swap happened, recursively max_heapify at this new i
```
-  For a given index $i$, we recurse `max_heapify(array, index)`  at most $h$ times (including first call), where $h$ is the largest height of the $i^{\text{th}}$ tree's subtrees
	- Each recurrence itself is $O(1)$, so altogether this is $O(h)\leq O(\text{height of tree})=O(\log_{2}n)$
- To build max-heap from an unsorted array $A$ of length $n$:
```Python
def build_max_heap(A)
	for i in (n // 2) to 1: 
	# This reverse order satisfies the precondition for max_heapify
	# Start with floor(n/2) because that's the first non-leaf node
		max_heapify(A, i)
```
- Note this is in-place; the input array is already given and is interpreted as a CBT (this algorithm makes no sense on an empty array; in that case use repeated insert which is $O(n\log_{2}n)$), we start with the first node of the lowest non-leaf level and bubble downward, iterating toward root 
	- This `build_max_heap` is trivially $\Omega(n)$ since the loop runs at most $\left\lfloor  \frac{n}{2}  \right\rfloor$ and each `max_heapify` could be (so this is a loose bound)
	- And $O(n\log_{2}n)$ is the upper bound by the same logic as above
	- But for a given node $x$, at depth $d$ each tree has $\leq 2^d$ nodes and height $\leq h-d$ where $h$ is the height of the entire tree; then build algorithm cost is:
$$
\begin{align}
&\leq \sum_{d=0}^{h-1}2^d \cdot (h-d) \\
& = \sum_{i=1}^h i\cdot 2^{h-i} \\
& = 2^h \cdot \sum_{i=1}^h \frac{i}{2^i} \\ \\
& \leq n \cdot \sum_{i=1}^\infty \frac{i}{2^i}
\end{align}
$$