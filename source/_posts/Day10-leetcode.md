---
title: Day10-leetcode
date: 2023-05-22 12:45:56
tags:
    - tree
    - double pointer
    - stack
    - hash_table
    - linked list
categories: data_structure
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---
# 今日题目：14, 141, 242, 496, 448, 100 
今天的题偏难，如果用最简单的方法去做的话。
# 14 字符串
题目：
> Write a function to find the longest common prefix string amongst an array of strings.
If there is no common prefix, return an empty string "".
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

代码：
```PYTHON
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if len(strs) == 1:
            return strs[0]
        rs = ''

        found_not_m = False

        for i in range(1, len(strs[0])+1):
            for _ in range(1, len(strs)):
                if strs[0][:i] != strs[_][:i]:
                    found_not_m = True 
                    break
                if _ == len(strs)-1:
                    rs = strs[0][:i]

            if found_not_m:
                    break
        return rs

```

# 141 Linked List Cycle
题目：
> Given head, the head of a linked list, determine if the linked list has a cycle in it.
There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.
Return true if there is a cycle in the linked list. Otherwise, return false.
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

最简单的做法是快慢指针，当两个进度不一样的指针循环时，总是会相撞，用这个来解决环路问题。
我的做法是升序赋值，可能会存在失效的问题。

代码：
```PYTHON
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        if not head or not head.next:
            return False
        
        slow = head
        fast = head.next
        
        while slow != fast:
            if not fast or not fast.next:
                return False
            slow = slow.next
            fast = fast.next.next
            
        
        return True

```

# 496 栈的运用
题目：
> The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.
You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.
For each 0 <= i < nums1.length, find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater element, then the answer for this query is -1.
Return an array ans of length nums1.length such that ans[i] is the next greater element as described above.
```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
```

这道题考察了对栈的运用。

代码：
```PYTHON
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        hash_table = {}
        stack = []
        for num in nums2:
            while stack and num > stack[-1]:
                hash_table[stack.pop()] = num
            stack.append(num)
        rs = []
        for num in nums1:
            if num in hash_table:
                rs.append(hash_table[num])
            else:
                rs.append(-1)
        return rs
```

# 448 原地哈希
题目：
> Given an array nums of n integers where nums[i] is in the range [1, n], return an array of all the integers in the range [1, n] that do not appear in nums.
```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```
这道题要求不能用额外空间，就是说不能直接用哈希表做，考的是用绝对值，**对一个值添加符号的方法就是添加绝对值。**

代码：
```PYTHON
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        rs = []
        for i, num in enumerate(nums):
            nums[abs(num) - 1] = abs(nums[abs(num) - 1])*(-1)

        for i, num in enumerate(nums):
            if num > 0:
                rs.append(i + 1)
        return rs

```

# 100 Same Tree

题目：
> Given the roots of two binary trees p and q, write a function to check if they are the same or not.
Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.
```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

两棵树很容易搞混。用元组来同时使用两个节点。

代码：
```PYTHON
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        if not p or not q:
            return False

        stack = [(p, q)]

        while stack:
            node1, node2 = stack.pop()

            if node1.val != node2.val:
                return False

            if node1.left and node2.left:
                stack.append((node1.left, node2.left))
            elif node1.left or node2.left:
                return False

            if node1.right and node2.right:
                stack.append((node1.right, node2.right))
            elif node1.right or node2.right:
                return False

        return True

```