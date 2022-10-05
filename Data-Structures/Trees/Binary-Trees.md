# Here are some problems covering Binary Trees

**They are usually solved using recursion**

**The stopping condition is usually an empty node** 

**The steps to solving any tree problem:**

1. Find one or more base cases
2. Call the same function on left subtree
3. Call the same function on right subtree
4. Join the results

We need to realize that the subtrees are also considered trees and the same function can be called upon them.

For example, to find the sum of a binary tree given the root 5:

It is 5 + the sum of the left subtree rooted at 11 + the sum of the right subtree rooted at 3
![Binary Tree Image](https://github.com/faa-99/Practicing-Interview-Questions/blob/main/Data-Structures/assets/binary-tree.png)

- Depth first traversal of a binary tree

    ```python
    """
              5
            /    \
            11     3
            /\     /\
            4 8    9 12
                    /\
                   6 15

    add the nodes as long as there are children
    when the leaf node is hit, remove to explore another path
    new node -> put it in the path -> go left -> no more children -> backtrack
    before backtracking, remove the actual node from path
    [5, 11, 4] -> remove 4
    [5, 11, 8] -> remove 8 and 11
    [5, 3, 9, 6] -> remove 6
    [5, 3, 9, 15] -> remove 15 and 9
    [5, 3, 12]
    need the parameter path:
        add the node at the beginning of the recursive case
        and remove it before backtracking
    """
    ```

- Invert a binary tree
    
    ```python
    def invert_binary_tree(root):
        """
        An inverted form of a Binary Tree is another Binary Tree with left 
        and right children of all non-leaf nodes interchanged. 
        You may also call it the mirror of the input tree.
        The root’s left pointer started pointing towards the right child 
        and the right pointer towards the left child 
        and a similar condition is noticed for all the sub root nodes.
        """
        if root is None:
            return root
    
    		root.left, root.right = root.right, root.left
    
        invert_binary_tree(root.left)
        invert_binary_tree(root.right)
    
        return root
    ```
    
- Merge 2 binary trees
    
    ```python
    def merge_trees(root1, root2):
        """
        Merge them by summing up their node values

        Time complexity : O(n) 
        A total of n nodes need to be traversed. Here, n represents the minimum 
        number of nodes from the two given trees.
        Auxiliary Space : O(n) 
        The depth of the recursion tree can go upto n in case of a skewed tree. 
        In average case, depth will be O(logn).
        """
        if not root1:
            return root2
        if not root2: 
            return root1
    
        root1.data += root2.data
        root1.left = merge_trees(root1.left, root2.left)
        root1.right = merge_trees(root1.right, root2.right)
        return root1
    ```
    
- Find if a node exists in tree and get the path from root
    
    ```python
    def get_path_from_root(root, target):
        def tree_includes(root, target, path):
        """
        performing dfs to find whether target node exists in the tree
        check if the target is the root or if it exists in the left subtree or right subtree
        """
            if root is None:
                        return False
    
            path.append(root)
            if root.data == target 
            or tree_includes(root.left, target, path) 
            or tree_includes(root.right, target, path):
                return True
            path.pop()
            return False
        path = []
        tree_includes(root, target, path)
        return path
    ```
    
- Find the kth ancestor of a node
    
    ```python
    def kth_ancestor(root, val, k):
        """
        Get the path from the root to the target node and return the kth ancestor
        """
        path = get_path_from_root(root, val)
        # ancestor does not exist
        if k >= len(path):
    	        return None
        # do the -1 because we need to skip the last element which is the
        # searched node itself
        else:
            return path[len(path)-k-1]
    ```
    
- Find min and max elements of a binary tree
    
    ```python
    def tree_min(root):
        if root is None:
            return float("inf")
        return min(root.data, tree_min(root.left), tree_min(root.right))
    
    def tree_max(root):
        if root is None:
            return -float("inf")
        return max(root.data, tree_min(root.left), tree_min(root.right))
    ```
    
- Sum of all nodes in binary tree
    
    ```python
    
    def tree_sum(root):
        """
        the root + sum of the left subtree + sum of the right subtree
        base case is when we reach a null node which returns a value of 0
        """
        if root is None:
            return 0
    
        return root.data + tree_sum(root.left) + tree_sum(root.right)
    ```
    
- Find the sum of each root-to-leaf path in binary tree
    
    ```python
    def tree_path_sums(root):
        def sum_root_to_leaf(root, value, sums):
        """
            for a tree 
                1
               / \
              2   3
             /\   /\
            4  5  6 7
                    \
                     8
        we give it the value of the root at each level

                                    1
                                /        \
                            1+2          1+3
                            /\            /\
                        1+2+4 1+2+5       6 7
                                            \
                                             8

        """
            if root is None:
                return 0
                # val = (val*10 + root.data)
            value += root.data
            if root.left is None and root.right is None:
                sums.append(value)
            sum_root_to_leaf(root.left, value, sums)
            sum_root_to_leaf(root.right, value, sums)
            return sums
    
        value = 0
        sums = []
        return sum_root_to_leaf(root, value, sums)
    ```
    
- Find all paths from **root to leaf** that sum up to target sum
    
    ```python
    def print_k_sum_paths(root, k):
        # print all paths that sum to k
        # keep track of nodes in each path and sum
        def find_paths(root, k, path_sum, path, paths):
        """
        For this problem, preorder traversal is best suited as we have to add up a 
        key value as we land on that node.

        We start from the root and start traversing by preorder traversal, adding key
        value to the path_sum and checking whether it is equal to the required sum when
        a leaf node is reached. 

        Also, as tree node doesn’t have a pointer pointing to its parent, we have to 
        explicitly save from where we have moved. We use a path to store the 
        path for this.

        Every node in this path contributes to the path_sum.
        """
            if root is None:
                return
            path_sum += root.data
            path.append(root.data)
    				# if we reach a leaf node and the sum of path is equal to target
            if root.left is None and root.right is None and path_sum == k:
                paths.append(path)
                return
    
            find_paths(root.left, k, path_sum, [] + path, paths)
            find_paths(root.right, k, path_sum, [] + path, paths)
            return paths
        return find_paths(root, k, 0, [], [])
    ```
    
- Fina all paths **from root** with a target sum
    
    ```python
    def print_k_sum_paths(root, k):
    	# print all paths starting from the root that sum to k
    	def find_paths(root, k, path_sum, path, paths):
            """
            For this problem, preorder traversal is best suited as we have to add up a 
            key value as we land on that node.

            We start from the root and start traversing by preorder traversal, adding key
            value to the path_sum and checking whether it is equal to the required sum. 

            Also, as tree node doesn’t have a pointer pointing to its parent, we have to 
            explicitly save from where we have moved. We use a path to store the 
            path for this.

            Every node in this path contributes to the path_sum.
            """
            if root is None:
                return

            path.append(root.data)
            path_sum += root.data
            
            if path_sum == k:
                paths.append(path)
                return

            find_paths(root.left, k, path_sum, [] + path, paths)
            find_paths(root.right, k, path_sum, [] + path, paths)
            path.pop()
    
            return paths
    
        return find_paths(root, k, 0, [], [])
    ```
    
- Find **all k-sum** paths in a binary tree
    
    ```python
    def print_any_k_sum_paths(root, k):
        # print all paths that sum to k
        # keep track of nodes in each path and sum
        def find_paths(root, k, path, paths):
            """
            for a tree 
                             1
                            / \
                           2   3
                              / \
                             6   7
            iteration 1:
            root  1
            path  1
            iteration 2:
            root  2
            path  1, 2
            iterations 3 and 4: root is NULL so return to the previous call and pass the
            right child of 2 which is also NULL
            root  NULL
            path  1, 2
            execution for root 2 is resumed, we take a variable path sum initialized to
            zero and start summing
            remove 2 from the path and resume execution for node 1 taking the right child
            """
            if root is None:
                return
    
            path.append(root.data)
    
            find_paths(root.left, k, [] + path, paths)
            find_paths(root.right, k, [] + path, paths)
    
            path_sum = 0
            for i in range(len(path) - 1, -1, -1):
                path_sum += path[i]
                if path_sum == k:
                    paths.append(path[i:])
    
            path.pop(-1)
    
            return paths
    
        return find_paths(root, k, [], [])
    ```
    
- Max root to leaf path sum
    
    ```python
    def max_root_to_leaf_path_sum(root):
        # each parent node will choose the max between its children
        if root is None:
            return -float("inf")
        if root.right is None and root.left is None:
            return root.data
    
        return root.data + max(
            max_root_to_leaf_path_sum(root.left), max_root_to_leaf_path_sum(root.right)
        )
    ```
    
- Find max height of a binary tree
    
    ```python
    def tree_depth(root):
        if root is None:
    			return 0
    		else:
    	    return 1 + max(tree_depth(root.left), tree_depth(root.right))
    ```
    
- Find max value in each level
    
    ```python
    def max_value_in_level(root):
        # return max value in each tree level
        # level-order traversal using bfs -> iterative with a queue
        if root is None:
            return []
        queue = [root]
        max_values = []
        while queue:
            max_value = -float("inf")
            nb_of_nodes = len(queue)
            for node_nb in range(1, nb_of_nodes + 1):
                current_node = queue.pop(0)
                if current_node.data > max_value:
                    max_value = current_node.data
                if current_node.left:
                    queue.append(current_node.left)
                if current_node.right:
                    queue.append(current_node.right)
            max_values.append(max_value)
    
        return max_values
    ```
    
- Find all nodes at a distance k from a start node
    
    ```python
    def nodes_distance_k(root, start_node, k):
        """
        the idea is to get the number of nodes that are k edges away from our start node
        we can view the tree as a graph and perform depth first search to get the 
        nodes that are k edges away
        """
        # need to find all nodes at distance k from a start node
        # nb of edges is k
        nodes = []
        graph = tree_to_graph(root)
        # bfs on graph
        queue = [(start_node, 0)]
        visited = set()
        visited.add(start_node)
        while queue:
            node = queue.pop(0)
            if node[1] == k:
                nodes.append(node[0].data)
            for neighbor in graph[node[0]]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append((neighbor, node[1] + 1))
        return nodes
    
    def tree_to_graph(root):
        # store every node as a key and the left, right, and parent as connections
        graph = {}
        queue = [root]
        while queue:
            current = queue.pop(0)
            if current not in graph:
                graph[current] = []
            if current.left:
                graph[current].append(current.left)
                if current.left not in graph:
                    graph[current.left] = []
                graph[current.left].append(current)
                queue.append(current.left)
            if current.right:
                graph[current].append(current.right)
                if current.right not in graph:
                    graph[current.right] = []
                graph[current.right].append(current)
                queue.append(current.right)
    
        return graph
    ```
    
- Find the lowest common ancestor between two nodes
    
    ```python
    def lowest_common_ancestor(root, node_1, node_2):
        """
        we have 3 scenarios that could occur:
        1- of the nodes is the root ie: an ancestor of the other and itself 
            => return the node
        2- both nodes belong to the same sub-tree
        3- each node belongs to a subtree
        If I am a root and I found one of the nodes to my right and one of the nodes
        to my left 
            => I am the common ancestor
        If I found one to my left but did not find any to my right or vice versa 
            => I am an ancestor but not the least common one.

        time complexity -> O(n) where n is nb of nodes
        space complexity -> O(h) where h is height of tree (stack call) 
        when tree is skewed

        case where one node is the parent of the other -> return that node
        case when they are in different subtrees
        """
        if root is None:
            return None
        if root.data == node_1.data or root.data == node_2.data:
            return root
        # drill downwards => need to find either node 1 or node 2
        left_node = lowest_common_ancestor(root.left, node_1, node_2)
        right_node = lowest_common_ancestor(root.right, node_1, node_2)
        # go upwards
        if left_node is None:
            return right_node
        if right_node is None:
            return left_node
        # searched left and found node, searched right and found node -> return myself
        return root
    ```
    
- Find if a tree is balanced or not (left and right subtrees of every node differ in height by no more than 1)
    
    ```python
    
    def check_balanced_tree(root):
        """
        for each of the nodes, I can check the height of the left subtree 
        and right subtree and compare them together.
        this would result in O(n^2)
        unneccesaarily calculate the height over and over again
        
        Optimized approach:
        all what we care about is the height of the node that we are sitting at 
        and whether its left and right subtrees are balanced.
        """
        def get_height(root):
            if root is None:
                return 0
            return max(get_height(root.left), get_height(root.right)) + 1
    
        if root is None:
            return True
        left_height = get_height(root.left)
        right_height = get_height(root.right)
    
        if (
                abs(left_height - right_height) >= 1
                and check_balanced_tree(root.left)
                and check_same_trees(root.right)
        ):
            return True
    
        return False
    ```
    
- Check if a tree is symmetric
    
    ```python
    def check_symmetric_tree(root):
    	"""
        given a binary tree, check if its a mirror of itself 
        (symmetric around its center).
        """
        def validate_symmetric(left, right):
            # check if either is null => we need them to both be null
            if left is None or right is None:
                return left == right
            # check if they have equal values
            if left.data != right.data:
                return False
            return validate_symmetric(left.left, right.right) 
                   and validate_symmetric(left.right, right.left)
    		# a null tree
        if root is None:
            return True
        # compare the subtrees to see if symmetric
        return validate_symmetric(root.left, root.right)
    ```
    
- Check if 2 trees are mirror trees
    
    ```python
    def check_mirror_trees(tree_1, tree_2):
        if tree_1 is None or tree_2 is None:
            return tree_1 == tree_2
        if tree_1.data != tree_2.data:
            return False
        return check_mirror_trees(tree_1.left, tree_2.right) and check_mirror_trees(
            tree_1.right, tree_2.left
        )
    ```
    
- Check if 2 trees are the same
    
    ```python
    def check_same_trees(tree_1, tree_2):
        # in terms of structure and values
        if tree_1 is None or tree_2 is None:
            return tree_1 == tree_2
        if tree_1.data != tree_2.data:
            return False
        return check_same_trees(tree_1.left, tree_2.left) and check_same_trees(
            tree_1.right, tree_2.right
        )
    ```
    
- Evaluate the expression of a tree

    ![Tree-Expression](https://github.com/faa-99/Practicing-Interview-Questions/blob/main/Data-Structures/assets/tree-evaluation.png)

    ```python
        def evaluate_tree(root):
            """
            all the integer values would appear at the leaf nodes, 
            while the interior nodes represent the operators.

            Therefore we can do inorder traversal of the binary tree 
            and evaluate the expression as we move ahead.
            """
            # empty tree
            if root is None:
                return 0

            # leaf node
            if root.left is None and root.right is None:
                return int(root.data)
        
            # evaluate left tree
            left_sum = evaluateExpressionTree(root.left)
        
            # evaluate right tree
            right_sum = evaluateExpressionTree(root.right)
        
            # check which operation to apply
            if root.data == '+':
                return left_sum + right_sum
        
            elif root.data == '-':
                return left_sum - right_sum
        
            elif root.data == '*':
                return left_sum * right_sum
        
            else:
                return left_sum // right_sum
    ```

- Left and Right Views of a tree

    ```python
    def left_view_tree(root):
        """
        print the left view of a binary tree
        first node of each level and keep track of each level
        whenever I see a node whose level is > level so far,
        print it -> go left then right
        """
        if root is None:
            return
        queue = [root]
        result = []
        while queue:
            # get number of nodes at current level
            nb_of_nodes = len(queue)
            for node_nb in range(1, nb_of_nodes + 1):
                current_node = queue.pop(0)
                if node_nb == 1:
                    result.append(current_node.data)
                if current_node.left:
                    queue.append(current_node.left)
                if current_node.right:
                    queue.append(current_node.right)
        return result

    def right_view_tree(root):
        """
        print the right view of a binary tree
        last node of each level and keep track of each level
        whenever I see a node whose level is > level so far,
        print it -> go left then right
        """
        if root is None:
            return
        queue = [root]
        result = []
        while queue:
            # get number of nodes at current level
            nb_of_nodes = len(queue)
            for node_nb in range(1, nb_of_nodes + 1):
                current_node = queue.pop(0)
                if node_nb == 1:
                    result.append(current_node.data)
                if current_node.right:
                    queue.append(current_node.right)
                if current_node.left:
                    queue.append(current_node.left)
        return result

    ```