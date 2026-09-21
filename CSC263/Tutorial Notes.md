BST deletes, node to be deleted $z$:
- When $z$ has $0$ children; simply delete the node and set the relevant pointer in its parent to null; this is $\Theta(1)$
- When $z$ has $1$ child (regardless of left or right); delete the node and set the parent of $z$ to now point directly to the child of $z$; this is $\Theta(1)$
- When $z$ has $2$ children; find the successor of $z$ (smallest key $>z$); this is done by going to the right child first, then repeatedly going to the left child until leaf 
	- Swap the successor into what was $z$ (this is still a valid BST at this point, but most use cases will not want duplicate data); then do delete algorithm (with $0\vee1$ child case, since the successor by definition has no left child) on successor; this is $\Theta(h)$