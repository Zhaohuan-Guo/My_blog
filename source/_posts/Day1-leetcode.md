---
title: Day1-leetcode
date: 2023-05-12 19:23:29
tags: 
    - data_structure
    - binary research
    - stack
    - sliding window
    - tree
    - BST
    - mirror tree
categories: data_structure
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---
# 今日题目：35， 155， 643， 501， 101，268

分别是二分插入，栈，滑动窗口，二叉搜索树，镜像二叉树，数学题。
难度都是简单，需要注意的地方：
1. 155栈的要求时间复杂度是O(1)，不能直接用min方法去获取值O(n)
2. 268太多解决方法了，注意它允许的额外空间只有O(1)，就不能用哈希表来解决了
# 35-二分插入
题目：
> Given a sorted array of distinct integers and a target value, return  the index if the target is found. If not, return the index where it would be if it were inserted in order.
You must write an algorithm with **O(log n)** runtime complexity.
```txt
Input: nums = [1,3,5,6], target = 2
Output: 1
```
 
二分插入可以递归和迭代，要会怎么递归转迭代：找到边界情况。
二分插入简而言之就是有左边界，中间值，右边界，当不断二分，三个值为一个值的时候，就在那个位置插入值，边界条件就是left = right = mid
代码：
```python
class Solution:
def searchInsert(self, nums: list[int], target: int) -> int:
    left = 0
    right = len(nums)

    while left < right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid

    return left
```
# 155 min stack
题目：
> Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
Implement the **MinStack** class:
**MinStack** initializes the stack object.
**void push** int val pushes the element **val** onto the stack.
**void pop** removes the element on the top of the stack.
**int top** gets the top element of the stack.
**int getMin** retrieves the minimum element in the stack.
You must implement a solution with **O(1)** time complexity for each function.

```txt
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```
唯一注意的就是需要辅助栈：
1. 进栈：辅助栈首大于进来的值，进辅助栈
2. 出栈：出栈的值是最小值，辅助栈出栈

代码：
```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self) -> None:
        if self.stack.pop() == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```
# 643 sliding window
题目：
> You are given an integer array **nums** consisting of **n** elements, and an integer **k**.
Find a contiguous subarray whose length is equal to **k** that has the maximum average value and return this value. Any answer with a calculation error less than **10^-5** will be accepted.
```txt
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
```
滑动窗口用来解决重复大量计算
代码：
```python
class Solution:
    def findMaxAverage(self, nums: list[int], k: int) -> float:
        curr_sum = sum(nums[0:k])
        max_avg = curr_sum / k

        for i in range(1, len(nums) - k + 1):
            curr_sum = curr_sum - nums[i - 1] + nums[i + k - 1]
            max_avg = max(max_avg, curr_sum / k)

        return max_avg
```

# 501 BTS
题目：
> Given the **root** of a binary search tree (BST) with duplicates, return all the **mode(s)** (i.e., the most frequently occurred element) in it.
If the tree has more than one mode, return them in any order.
Assume a BST is defined as follows:
-The left subtree of a node contains only nodes with keys less than or equal to the node's key.
-The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
-Both the left and right subtrees must also be binary search trees.
 
```txt
Input: root = [1,null,2,2]
Output: [2]
```
## 首先介绍一下三种遍历方法：
1. Pre-order traversal: In pre-order traversal, the **root node** is visited first, followed by visiting the **left** subtree recursively, and then visiting the **right** subtree recursively. The order of visiting the nodes is: **root, left subtree, right subtree.**
2. In-order traversal: The order of visiting the nodes is: **left subtree, root, right subtree.**
3. Post-order traversal： The order of visiting the nodes is: **left subtree, right subtree, root.**


## 还有他们的特点：
- Pre-order traversal:
- Features:
    - Visits the nodes in a top-down manner.
    - Root node is visited first.
    - Useful for creating a copy of the tree, evaluating expressions, and prefix notation.
- Applications:
    - Constructing a clone of the tree.
    - Expression evaluation (prefix notation).
    - Finding the path from the root to any given node.
- In-order traversal:
    - Features:
        - Visits the nodes in a sorted order (**ascending order for binary search trees**).
        - Useful for retrieving nodes in non-decreasing order.
    - Applications:
        - Retrieving nodes in a sorted order.
        - Validating the structure and properties of a binary search tree.
        - Finding the middle element of a binary search tree.
- Post-order traversal:
    - Features:
        - Visits the nodes in a bottom-up manner.
        - Useful for deleting the tree, computing post-fix notation, and calculating the height of a tree.
    - Applications:
        - Deleting the entire tree.
        - Evaluating expressions (post-fix notation).
        - Calculating the height of a tree.
## 解题
在了解他们的特性之后，求所有众数的思路就是在中序遍历的时候，一样的数是挨在一起的，然后当数变了就重新计数。
需要 初始化 这几个参数：
```txt
        curr_val = None
        max_count = 0
        curr_count = 1
        modes = []
```
- curr_val变化的时候，curr_count变化。
- curr_count > max_count 时候， modes清空。
- curr_count == max_count 时候，入栈。
代码：
```python
from typing import Optional
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def findMode(self, root: Optional[TreeNode]) -> list[int]:
        curr_val = None
        max_count = 0
        curr_count = 1
        modes = []

        def count(val):
            nonlocal curr_val, max_count, curr_count
            if val == curr_val:
                curr_count += 1
            else:
                curr_val = val
                curr_count = 1
            if curr_count > max_count:
                max_count = curr_count
                modes.clear()
                modes.append(curr_val)
            elif curr_count == max_count:
                modes.append(curr_val)

        def inorderTraversal(node):
            if node is None:
                return
            inorderTraversal(node.left)
            count(node.val)
            inorderTraversal(node.right)

        inorderTraversal(root)
        return modes
```
# 101 对称树
题目：
> Given the **root** of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).
 
```txt
Input: root = [1,2,2,3,4,4,3]
Output: true
```
这题就是考验递归
```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        def check_mirror(l, r):
            if not l and not r:
                return True
            if not l or not r or l.val != r.val:
                return False
            return check_mirror(l.left, r.right) and check_mirror(l.right, r.left)
        return check_mirror(root.left, root.right)
```
递归改迭代写法：
ps: 就是把所有要对比的放在一起，然后全部对比。
```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        
        queue = deque()
        queue.append((root.left, root.right))
        
        while queue:
            left, right = queue.popleft()
            
            if not left and not right:
                continue
            
            if not left or not right or left.val != right.val:
                return False
            
            queue.append((left.left, right.right))
            queue.append((left.right, right.left))
        
        return True
```
# 268 数学题
题目：
> Given an array **nums** containing **n** distinct numbers in the range **[0, n]**, return the only number in the range that is missing from the array.
 
```txt
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```
```python
class Solution:
    def missingNumber(self, nums: list[int]) -> int:
        l = len(nums)
        return (1+l)*l//2-sum(nums)
```