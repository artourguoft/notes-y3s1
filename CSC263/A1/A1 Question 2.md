## <u>Algorithm Description</u>
The algorithm accumulates **only the integer key inputs** into a locally declared **max-heap**, between PRINT operation inputs
- The integers are accumulated using a standard `insert()` operation implementation for max-heaps, until the max-heap size reaches $m$
- Once the max-heap reaches a size of $m$ nodes, we compare each incoming key with the current maximum key in the max-heap; this is done with the standard `find_max()` implementation
	- If the incoming key is smaller than this maximum, we replace the maximum with the new key, which is done by simply putting the new key into the root node directly
		- This is because the incoming key is then by definition one of the $m$ smallest key inputs that we have seen so far, so we need to store it for future retrieval when printing the $m$ smallest inputs so far 
		- Then we call the recursive `max_heapify()` helper on the root node to restore the max-heap property of the max-heap before continuing to the next loop iteration
		- Note that the max-heap property is not sufficient to allow us to retrieve the keys from the max-heap in increasing or decreasing order, but this is not a problem as our algorithm is only asked to print the $m$ smallest keys **in any order**
	- If the incoming key is greater than or equal to this maximum, we do nothing and simply move to the next iteration over the inputs stream
		- This is because the incoming key is then by definition **not** one of the $m$ smallest key inputs that we have seen so far, and thus we don't need to store it for any future reference
- When a PRINT input is encountered when looping over the inputs stream, we simply print the entire contents of the max-heap by looping over it and printing each key 
## <u>Pseudocode</u>
```Python
algo(m, inputs[1...])
	max_heap = []                               # locally declare a max-heap as an empty dynamic array 
	for i in inputs                                
		if (i == PRINT)
			for j in max_heap
				print(j)
	    else if (i is int)
		    if (max_heap.size < m)              # if the max-heap has < m keys, we simply insert the new key    
			    insert(max_heap, i)         
			else if (i < find_max(max_heap))    # if the max-heap has m keys AND the new key is < its max
				max_heap[1] = i                 # we replace the root (max) with the new key
				max_heapify(max_heap, 1)        # then bubble down the new key till max-heap property satisfied 
	                                            # else we do nothing with the input and go next iteration

### Helper functions below 
 
insert(A, x)
  A[A.size + 1] = x                              
  A.size++                                       
  j = A.size                                     
  while ((j > 1) and (A[j] > A[j // 2]))         
      temp = A[j]
      A[j] = A[j // 2]
      A[j // 2] = temp                           
      j = j // 2                                

find_max(A)
    return A[1]
    
max_heapify(A, k)
    left  = 2 * k
    right = 2 * k + 1
    if ((left <= A.size) and (A[left] > A[k]))
        largest = left
    else 
	    largest = k
	if ((right <= A.size) and (A[right] > A[largest]))
        largest = right
    if (largest != k) 
	    temp = A[k]
	    A[k] = A[largest]
	    A[largest] = temp
	    k = largest
	    max_heapify(A, largest)
```
## <u>Runtime Complexity</u>
- When processing key inputs, we have two conditional scenarios:
	- Max-heap size $<m$:
		- In this case we simply call the standard `insert()` operation
		- This is runtime class $O(\log_{2}m)$ since the max-heap height is $\leq\lfloor \log_{2}m \rfloor$ since max-heap size is $<m$; $\lfloor \log_{2}m \rfloor$ is the largest number of times that the algorithm may have to percolate the newly inserted key value upward (from leaf to root, if new key is new maximum)
	- Max-heap size $= m$:
		- Call `find_max(max_heap)` which directly returns the first element of the array, which is $O(1)$
		- If the new key is $\geq$ `find_max(max_heap)`, do nothing
		- If the new key is $<$ `find_max(max_heap)`:
			- Replace the root with the new key directly through the first element of the array, which is $O(1)$
			- Then call the recursive `max_heapify()` helper on the root node; this helper is runtime class $O(\log_{2}m)$ since:
				- Each parent-child key comparison and possible swap is $O(1)$ 
				- Worst-case we may have to bubble down the newly inserted key value downward from root to leaf; so the largest number of times this function may have to recurse is the height of the max-heap, and the height is $\lfloor \log_{2}m \rfloor$
	- Thus, in either scenario the processing of a key input is $O(\log_{2}m)$
- When processing a PRINT input, we assume each print operation is $O(1)$ and loop that operation over the entire array representing the max-heap
	- We are given the precondition that the first PRINT is assumed to encountered after at least $m$ keys have been processed, so the max-heap array will always have exactly $m$ elements to print, so this always yields a runtime of $O(m)$
## <u>Correctness</u>
Define a **loop invariant** $I$ to state that before any iteration of the loop, the max-heap holds the $\min(m,k)$ smallest input keys of the $k$ input keys processed so far
- **Base Case:** before any iterations, $I$ states that the max-heap holds the $\min(m,0)$ smallest keys of the $0$ keys processed so far, which is true; $\min(m,0)=0$ since $m\geq 1$ by precondition, and the max-heap holds $0$ keys at this point as needed
- **Induction:** assume the induction hypothesis $I$ as stated above, then during the upcoming iteration there are several scenarios:
	- The input during this iteration is a PRINT, in which case $I$ remains true as there are no changes to the max-heap and no changes to $k$
	- The input during this iteration is a key, and:
		- $k+1\leq m$, which means the max-heap before this iteration has $<m$ nodes, so the $k+1^{\text{st}}$ key will be inserted into the max-heap and will by definition be one of the $\min(m,k+1)$ smallest keys processed so far (regardless of whether $k+1=m$ or still $k+1<m$)
		- $k+1> m$, which means the max-heap before this iteration has $m$ nodes, so the $k+1^{\text{st}}$ key will be inserted into max-heap only if it is less than the current maximum of the max-heap
			- If it is inserted, then we replaced the largest of the previously smallest keys with a smaller key, so by definition the max-heap now holds the new $\min(m,k+1)=m$ smallest keys of the $k+1$ keys processed, so $I$ remains true
			- If it is not inserted, then by $I$ the max-heap still holds the current $m$ smallest keys, but now of the $k+1$ keys processed
- Thus, we have shown the $I$ always holds - so when a PRINT input is encountered, which by precondition is after at least $m$ key inputs have been processed, the max-heap which is printed will hold precisely the $m$ smallest keys processed so far
