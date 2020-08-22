# Classified Problem List

**Note: This note is based on [Huahua's Tech Road](https://zxi.mytechroad.com/blog/).**

### Tree

##### 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

Similar Problems: [144](https://leetcode.com/problems/binary-tree-preorder-traversal/), [145](https://leetcode.com/problems/binary-tree-postorder-traversal/), [429](https://leetcode.com/problems/n-ary-tree-level-order-traversal/), [589](https://leetcode.com/problems/n-ary-tree-preorder-traversal/), [590](https://leetcode.com/problems/n-ary-tree-postorder-traversal/), [987](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/), [1302](https://leetcode.com/problems/deepest-leaves-sum/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

# 94. Binary Tree Inorder Traversal

# Recursive Solution
# Time: O(N)
# Space: O(N)
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        return [] if not root else self.inorderTraversal(root.left)+[root.val]+self.inorderTraversal(root.right)

# Iterative Solution
# Time: O(N)
# Space: O(N)
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        stack = []
        while True:
            while root:
                stack.append(root)
                root = root.left
            if not stack:
                return result
            node = stack.pop()
            result.append(node.val)
            root = node.right
            
# 144. Binary Tree Preorder Traversal

# Recursive Solution
# Time: O(N)
# Space: O(N)
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        return [] if not root else [root.val]+self.preorderTraversal(root.left)+self.preorderTraversal(root.right)
      
# Iterative Solution 1
# Time: O(N)
# Space: O(N)
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        stack = [root]
        ans = []
        while stack:
            node = stack.pop()
            ans.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return ans
      
# Iterative Solution 2
# Time: O(N)
# Space: O(N)
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                result.append(node.val)
                stack.append(node.right)
                stack.append(node.left)
        return result
      
# 145. Binary Tree Postorder Traversal

# Iterative Solution
# Time: O(N)
# Space: O(logN)
# Idea: Reverse a preorder traversal.
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        stack = [root]
        ans = []
        while stack:
            node = stack.pop()
            if node:
                ans.append(node.val)
                stack.append(node.left)
                stack.append(node.right)
        return ans[::-1]
      
# Iterative Solution
# Time: O(N)
# Space: O(logN)
# Idea: Preorder traverse, use stack to adjust the order of appending the answer.
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        stack = []
        ans = []
        while root or stack:
            while root:
                if root.right:
                    stack.append(root.right)
                stack.append(root)
                root = root.left
            
            root = stack.pop()
            
            if stack and root.right == stack[-1]:
                stack[-1] = root
                root = root.right
            else:
                ans.append(root.val)
                root = None
        return ans
      
# 429. N-ary Tree Level Order Traversal

# Iterative Solution
# Time: O(N)
# Space: O(W) W is the width of the tree.
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        level = [root]
        ans = []
        while level:
            l_ans = []
            tmp = []
            for node in level:
                if node:
                    l_ans.append(node.val)
                    for child in node.children:
                        tmp.append(child)
            level = tmp
            ans.append(l_ans)
        return ans

# Recursive Solution
# Time: O(N)
# Space: O(N)
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        ans = []
        
        def BFS(node, level):
            if len(ans) == level:
                ans.append([])
            ans[level].append(node.val)
            for child in node.children:
                BFS(child, level+1)
                
        BFS(root, 0)
        return ans
      
# 589. N-ary Tree Preorder Traversal

# Recursive Solution
# Time: O(N)
# Space: O(N)
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        if not root:
            return []
        nxt = []
        for child in root.children:
            nxt += self.preorder(child)
        return [root.val] + nxt
      
# Iterative Solution
# Time: O(N)
# Space: O(logN)
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        ans = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                ans.append(node.val)
                for child in reversed(node.children):
                    stack.append(child)
        return ans
      
# 590. N-ary Tree Postorder Traversal

# Iterative Solution
# Time: O(N)
# Space: O(logN)
class Solution:
    def postorder(self, root: 'Node') -> List[int]:
        ans = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                ans.append(node.val)
                for child in node.children:
                    stack.append(child)
        return ans[::-1]
      
# 987. Vertical Order Traversal of a Binary Tree

# Store level result in dict
# Time: O(NlogN)
# Space: O(N)
class Solution:
    def verticalTraversal(self, root: TreeNode) -> List[List[int]]:
        verticalLevel = {}
        level = [[root, 0]]
        while level:
            tmp = []
            dic = {}
            for node, ver in level:
                if ver not in dic:
                    dic[ver] = [node.val]
                else:
                    dic[ver].append(node.val)
                if node.left:
                    tmp.append([node.left, ver-1])
                if node.right:
                    tmp.append([node.right, ver+1])
            for key in dic.keys():
                if key in verticalLevel:
                    verticalLevel[key].extend(sorted(dic[key]))
                else:
                    verticalLevel[key] = sorted(dic[key])
            level = tmp
        ans = []
        for ver in sorted(verticalLevel.keys()):
            ans.append(verticalLevel[ver])
        return ans
      
# 1302. Deepest Leaves Sum

# Iterative level order traversal
# Time: O(N)
# Space: O(N)
class Solution:
    def deepestLeavesSum(self, root: TreeNode) -> int:
        queue = [root]
        while queue:
            tmp = []
            ans = 0
            for node in queue:
                ans += node.val
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            queue = tmp
        return ans
```

