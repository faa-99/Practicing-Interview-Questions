**They are usually solved using recursion**

**The stopping condition is usually an empty node** 

**The steps to solving any tree problem:**

1. Find one or more base cases
2. Call the same function on left subtree
3. Call the same function on right subtree
4. Join the results

We need to realize that the subtrees are also considered trees and the same function can be called upon them.

For example, to find the sum of a binary tree given the root 5:

it is 5 + the sum of the left subtree rooted at 11 + the sum of the right subtree rooted at 3
![This is an image](https://github.com/faa-99/Practicing-Interview-Questions/Data-Structures/assets/binary-tree.png)