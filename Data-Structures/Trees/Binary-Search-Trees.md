# Questions Regarding BST

- Validate if a tree is a binary search tree
    
    ```python
    def validate_bst(root):
        """
        maintain a min and max and check if each node satisfies the boundary
        we traversed all nodes, and they satisfy bst
        """
        def valid(root, min_value, max_value):
            # an empty binary search tree is a valid bst
            if root is None:
                return True
                
            if not (min_value <= root.data <= max_value):
                return False 

            return valid(root.left, min_value, root.data) and
                    valid(root.right, root.data, max_value)

        return valid(root, float("-inf"), float("inf")) 
    ```
    
- Convert a binary search tree to an ordered doubly linked list
    
    ```python
    
    def convert_bst_to_dll(root):
        """
        Think of the left and right pointers as synonymous to the previous and next 
        pointers in a doubly-linked list since they resemble an element which is less than
        the current node value and an element greater than it.
        
        the in-order traversal of a bst will return it in ascending order (left, root, right)
        """
        if root is None:
            return
        convert_bst_to_dll(root.left)
        if prev is None:
            head = root
        # for every node, we need to assign the previous value and the next value
        else:
            root.left = prev
            prev.right = root
        prev = root
        convert_bst_to_dll(root.right)
    	
    ```
    
- Return the kth smallest element in a binary search tree
    
    ```python
    def kth_smallest_element_in_bst(root, k):
        """
        in-order traversal of bst returns it in ascending order
        """
        count = 0
        stack = []
        current = root
            while True:
                if current is not None:
                    stack.append(current)
                    current = root.left
                elif stack:
                    current = stack.pop()
                    count += 1
                    if count == k:
                        return current.data
                    current = current.right
                else:
                    break
    ```
    
- Find a node in a binary search tree whose value is closest to a number k
    
    ```python
    def closest_value(root, k):
        current = root
        min_value = float("Inf")
        closest_node = root.data
        while current is not None:
            # get the difference between the current and the target
            difference = abs(current.data - k)
            # check if the difference is the minimum difference
            if difference <= min_value:
                min_value = difference
                closest_node = current.data
            # check whether to look in right subtree or left subtree
            if k < current.data:
                # look in left subtree
                current = current.left
            elif k > current.data:
                # look in right subtree
                current = current.right
        return min_value		
    ```

- Convert to greater tree
    
    ```python
    def convert_greater_tree(root):
    	"""
    	for every element => add the sum of nodes in the right subtree
    	we can dfs the right subtree for every node => not very efficient since we will be 
    	repeating ourselves
    	
    	Optimal way: in order traversal would be the left subtree, root, then right subtree
    	we want the exact opposite
    	to get the vale of each node => sum its right subtree
    	we need to perform inorder traversal in reverse order
    	at each step, get the right sum, add to root, then add them to left node
    	keep track of the sum so far
    	O(n) time and O(h) space
    	"""
    	def dfs(root, curr_sum):
    		# perform reverse in order traversal
    		if root is None:
    			return
    		dfs(root.right, curr_sum)
    		temp = root.val
    		root.val += curr_sum
    		curr_sum += temp
    		dfs(root.left) 
    
    		return root
    
    	dfs(root, 0)
    	return root
    ```
- Find a pair with a given sum in Binary search tree
- Search for an element