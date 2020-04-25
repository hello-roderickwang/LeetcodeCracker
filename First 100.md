### 1. [Two Sum](https://leetcode.com/problems/two-sum/)

**First Solution***

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hash = {}
        for index, num in enumerate(nums):
            remain = target - num
            if remain not in hash:
                hash[num] = index
            else:
                return [hash[remain], index]
```

Notes:

1. The enumerate() function can get value and index at same time;
2. Use hash-table(dict() in python) to save time.



### 2. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

**First Solution***

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        result = ListNode(0)
        result_tail = result
        carry = 0
        while l1 or l2 or carry:
            val1 = (l1.val if l1 else 0)
            val2 = (l2.val if l2 else 0)
            carry, out = divmod(val1+val2+carry, 10)
            result_tail.next = ListNode(out)
            result_tail = result_tail.next
            l1 = (l1.next if l1 else None)
            l2 = (l2.next if l2 else None)
        return result.next
```

Notes:

1. Usage of divmod() function;
2. Define a dummy head(result) to initiate loop.



### 3. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**First Solution**

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        length = len(s)
        hashtable = {}
        num = ([0] if length==0 else [1])
        ctr = 0
        i = 0
        while i < length:
            if s[i] in hashtable:
                num.append(ctr)
                ctr = 0
                i = hashtable[s[i]] + 1
                hashtable.clear()
                hashtable[s[i]] = i
                ctr += 1
                i+=1
            else:
                ctr += 1
                hashtable[s[i]] = i
                i+=1
        num.append(ctr)
        return max(num)
```

Notes:

1. Remember to consider null string and None.



### 4. [3Sum](https://leetcode.com/problems/3sum/)

**First Solution***

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        length = len(nums)
        result = []
        for i in range(length):
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i-1]:
                continue
            l, r = i+1, length-1
            while l < r:
                total = nums[i]+nums[l]+nums[r]
                if total < 0:
                    l += 1
                elif total > 0:
                    r -= 1
                else:
                    result.append([nums[i], nums[l], nums[r]])
                    while l<r and nums[l]==nums[l+1]:
                        l+=1
                    while l<r and nums[r]==nums[r-1]:
                        r-=1
                    l+=1
                    r-=1
        return result
```

Notes:

1. Two moving pointer search in a hash-table.

Explanation:

The main idea is to iterate every number in nums. We use the number as a target to find two other numbers which make total zero. For those two other numbers, we move pointers, l and r, to try them.

l start from left to right. r start from right to left.

First, we sort the array, so we can easily move i around and know how to adjust l and r. If the number is the same as the number before, we have used it as target already, continue.[1] We always start the left pointer from i+1 because the combination of 0~i has already been tried.[2]

Now we calculate the total:
If the total is less than zero, we need it to be larger, so we move the left pointer.[3] If the total is greater than zero, we need it to be smaller, so we move the right pointer.[4] If the total is zero, bingo![5] We need to move the left and right pointers to the next different numbers, so we do not get repeating result.[6]

We do not need to consider i after nums[i]>0, since sum of 3 positive will be always greater than zero.[7] We do not need to try the last two, since there are no rooms for l and r pointers. You can think of it as The last two have been tried by all others.[8]

For time complexity
Sorting takes O(NlogN). Now, we need to think as if the nums is really really big. We iterate through the nums once, and each time we iterate the whole array again by a while loop. So it is O(NlogN+N^2)~=O(N^2)

For space complexity
 We didn't use extra space except the res. So it is O(1).



### 5. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

**First Solution (Recursive)**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        def preorder(node):
            if node:
                result.append(node.val)
                preorder(node.left)
                preorder(node.right)
        preorder(root)
        return result
```

**Second Solution (Iterative)**

```python
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
```



### 6.[Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

**First Solution (Recursive)**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        def inorder(node):
            if node:
                inorder(node.left)
                result.append(node.val)
                inorder(node.right)
        inorder(root)
        return result
```

**Second Solution (Iterative)**

```python
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
```



### 7. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

**First Solution (Recursive)**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        def postorder(node):
            if node:
                postorder(node.left)
                postorder(node.right)
                result.append(node.val)
        postorder(root)
        return result
```

**Second Solution (Iterative)**

```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                # pre-order, right first
                result.append(node.val)
                stack.append(node.left)
                stack.append(node.right)
        # reverse result
        return result[::-1]
```



### 8. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

**First Solution**

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        length1 = len(nums1)
        length2 = len(nums2)
        head1 = head2 = 0
        tail1 = length1 - 1
        tail2 = length2 - 1
        # state1 = state2 = 1
        state1 = (0 if length1 == 0 else 1)
        state2 = (0 if length2 == 0 else 1)
        while state1 or state2:
            while not state1:
                if head2 == tail2:
                    return nums2[head2]
                head2 += 1
                tail2 -= 1
                if head2 > tail2:
                    return (nums2[head2]+nums2[tail2])/2
            while not state2:
                if head1 == tail1:
                    return nums1[head1]
                head1 += 1
                tail1 -= 1
                if head1 > tail1:
                    return (nums1[head1]+nums1[tail1])/2
                
            if head1 == tail1 and head2 == tail2:
                return (nums1[head1]+nums2[head2])/2
                
            if nums1[head1]<=nums2[head2]:
                head1 += 1
            else:
                head2 += 1
                
            if nums1[tail1]>=nums2[tail2]:
                tail1 -= 1
            else:
                tail2 -= 1
                
            if head1 > tail1:
                state1 = 0
                
            if head2 > tail2:
                state2 = 0
```

Notes:

1. For each array, we set two pointer(head&tail);
2. Compare heads and tails, basically to sort these two array and take the median;
3. Time complexity is wrong, this solution is O($m+n$) not O($\log(m+n)$) as asked.



### 9. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

**First Solution***

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        ret = ''
        length = len(s)
        for i in range(length):
            for j in range(length, i, -1):
                if len(ret) >= j-i:
                    break
                elif s[i:j] == s[i:j][::-1]:
                    ret = s[i:j]
                    break
        return ret
```

Notes:

1. Brute force solution;
2. Time complexity: O($n^3$)
3. Take a look at [Manacher's Algorithm](https://www.geeksforgeeks.org/manachers-algorithm-linear-time-longest-palindromic-substring-part-1/)

