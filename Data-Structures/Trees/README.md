T**hey provide a means to store data hierarchically**. 

Used for searching and sorting

We can search for given key in a moderate amount of time (Quicker than Linked Lists and slower than Arrays).

Don’t have an upper limit on number of nodes as nodes are linked using pointers.

Some common operations that can be conducted on trees include insertion and deletion in moderate time.

### Binary Trees

Each node has up to 2 children

1. Complete Binary Tree: every level is fully filled except maybe the last where it is filled left to right
2. Full Binary Tree: every node has either 0 or 2 children
3. Perfect Binary Tree: All interior nodes have 2 children and all leaf nodes are at the same level

### Binary Search Trees

All left descendants ≤ node < all right descendants

This should be true for all node descendants

Both left and right subtrees are also BSTs

- Optimizes searching for an element
- It is helpful in maintaining a sorted stream of data.

### Traversals

Implement a tree iterator

DFS: depth first search

**In order traversal:** recursive and iterative

LEFT ROOT RIGHT

when we want to flatten the tree back into its original sequence

```python
def in_order_traversal(root):
    if root is None:
        return []

    return in_order_traversal(root.left) + [root.data] + in_order_traversal(root.right)

def in_order_iterative(root):
"""
    only when we pop the node do we want to visit its right child

    """
		current = root
    stack = []
    result = []
    while True:
        # go deep to the left
        if current is not None:
            stack.append(current)
            current = root.left
        # reached the leaf node -> print the value
        elif stack:
            current = stack.pop()
            result.append(current.data)
            current = current.right
        else:
            break
```

**Post order traversal:** recursive and iterative

LEFT RIGHT ROOT

when we want to explore the leaves before the nodes

```python
def post_order_traversal(root):
    if root is None:
        return []
    return (
            post_order_traversal(root.left) + post_order_traversal(root.right) + [root.data]
    )

def post_order_iterative(root):
    pass
```

**Pre order traversal:** recursive and iterative

ROOT LEFT RIGHT

when we want to explore the roots before the leaves

```python
def pre_order_traversal(root):
    if root is None:
        return []
    return (
        [root.data] + pre_order_traversal(root.left) + pre_order_traversal(root.right)
    )
```

**Level order traversal (breadth-first): iterative**

Traverses nodes at each level before going to the next level

```python
def breadth_traversal(root):
    queue = [root]
    result = []
    while queue:
        node = queue.pop(0)
        result.append(node.data)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return result
```