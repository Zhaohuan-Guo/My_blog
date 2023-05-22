---
title: Day8-9-leetcode
date: 2023-05-21 01:17:03
tags:
    - tree
    - dynamic planning
    - double pointer
    - queue
    - stack
    - hash_table
    - linked list
categories: data_structure
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
mathjax: true
---
# 两日题目：392, 111, 589,  58, 303,  169, 108, 217, 283, 118, 1332, 21
# 392 Is Subsequence
题目：
> Given two strings s and t, return true if s is a subsequence of t, or false otherwise.
A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

```
Input: s = "abc", t = "ahbgdc"
Output: true
```

用队列来解决。

代码：
```PYTHON
from collections import deque


class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if not s:
            return True
        queue = deque([])
        for char in s:
            queue.append(char)

        for char in t:
            if not queue:
                return True
            if char == queue[0]:
                queue.popleft()

        if not queue:
            return True
        else:
            return False
```


# 111 二叉树最小深度
题目：
> Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
Note: A leaf is a node with no children.

```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
```

依旧使用队列来解决，并且要将层数与节点进行绑定。
这道题beat100%!

代码：
```PYTHON
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        queue = deque([(root, 1)])
        rs = 0
        node = root
        while node:
            node, node.level = queue.popleft()
            rs = node.level
            if not node.left and not node.right:
                return rs
            if node.left:
                queue.append((node.left, node.level + 1))
            if node.right:
                queue.append((node.right, node.level + 1))
```


# 589 前序遍历
题目：
> Given the root of an n-ary tree, return the preorder traversal of its nodes' values.
Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [1,3,5,6,2,4]
```

迭代法解决前序。

代码：
```PYTHON
class Solution:
    def preorder(self, root: 'Node') -> List[int]:
        if not root:
            return []
        list_ = [root]
        rs = []
        while list_:
            node = list_.pop()
            rs.append(node.val)
            for i in range(len(node.children) - 1, -1, -1):
                list_.append(node.children[i])
        return rs
            return False
```


# 58
题目：
> Given a string s consisting of words and spaces, return the length of the last word in the string.
A word is a maximal substring consisting of non-space characters only.

```
Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.
```

莫名其妙的题目

代码：
```PYTHON
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.split()[-1])
```


# 303 动态规划
题目：
> Given an integer array nums, handle multiple queries of the following type:
Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:
NumArray(int[] nums) Initializes the object with the integer array nums.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).

```
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

动态规划的题，要保证时间复杂度。

代码：
```PYTHON
class NumArray:

    def __init__(self, nums: List[int]):
        self.prefix_sum = [0]
        for num in nums:
            self.prefix_sum.append(self.prefix_sum[-1] + num)

    def sumRange(self, left: int, right: int) -> int:
        return self.prefix_sum[right+1] - self.prefix_sum[left]
```


# 169 摩尔投票
题目：
> Given an array nums of size n, return the majority element.
The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

这道题有点难噢，摩尔投票法。

摩尔投票法（Moore's Voting Algorithm）是一种用于在给定数组中寻找出现次数超过一半的元素的有效算法。这个算法的思想简单而巧妙，可以在O(n)的时间复杂度和O(1)的空间复杂度下解决问题。

摩尔投票法的基本思想如下：

初始化两个变量 candidate 和 count，分别用于保存候选元素和计数器，初始值可以为任意值。
遍历数组，对于每个元素：
如果计数器 count 为0，将当前元素设为候选元素 candidate。
如果当前元素与候选元素相同，将计数器 count 加1。
如果当前元素与候选元素不同，将计数器 count 减1。
最终剩下的候选元素就有很大概率是出现次数超过一半的元素。
该算法的核心思想是利用了出现次数超过一半的元素的特性：它出现的次数比其他所有元素的次数之和还要多。通过遍历数组，将不同的元素两两抵消，最终剩下的元素即为候选元素。由于候选元素出现的次数超过一半，因此最终剩下的候选元素即为我们要找的结果。

需要注意的是，摩尔投票法的前提是输入数组中一定存在出现次数超过一半的元素。在应用该算法时，我们应该先确保题目给定的条件满足这个前提。

摩尔投票法在解决出现次数超过一半的元素的问题上非常高效，并且可以轻松地扩展到寻找出现次数超过1/k的元素，其中k为正整数。

重点：**两两抵消**

代码：
```PYTHON
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        candidate = nums[0]
        count = 1
        
        for i in range(1, len(nums)):
            if nums[i] == candidate:
                count += 1
            else:
                count -= 1
                if count == 0:
                    candidate = nums[i]
                    count = 1
        
        return candidate
```


# 108 二叉树+二分法
题目：
> Given an integer array nums where the elements are sorted in ascending order, convert it to a 
height-balanced binary search tree.

```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:
```

这道二叉树与二分法结合的题目也不简单，主要涉及到二分法二分时候会有小数的问题。

代码：
```PYTHON
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        right = len(nums) - 1
        left = 0        
        
        def build_tree(nums, left, right):
            if left > right:
                return 
            mid = (left + right) // 2
            root = TreeNode(nums[mid])
            root.left = build_tree(nums, left, mid - 1)
            root.right = build_tree(nums, mid + 1, right)

            return root
        

        return build_tree(nums, left, right )
```


# 217 哈希表
题目：
> Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.
```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

经典哈希表。

代码：
```PYTHON
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hash_table = {}
        for num in nums:
            hash_table[num] = hash_table.get(num, 0) + 1
            if hash_table[num] == 2:
                return True

        return False 
```


# 283 双指针
题目：
> Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.
Note that you must do this in-place without making a copy of the array.

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

涉及到指针的变化，双指针。

代码：
```PYTHON
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        count = 0
        l = len(nums)
        j = 0
        for i in range(l):
            if nums[i] == 0:
                count += 1
                continue
            j = i - count
            nums[j] = nums[i]
        for i in range(j+1, l):
            nums[i] = 0
```

------------------------------------------------------
# 118 杨辉三角
题目：
> Given an integer numRows, return the first numRows of Pascal's triangle.
In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

```
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

动态规划之简单版杨辉三角。复杂版的要考虑空间复杂度。


代码：
```PYTHON
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        if not numRows:
            return []
        rs = [[1]]
        for i in range(1, numRows):
            rs.append([])
            rs[i].append(1)
            if i > 1:
                for j in range(i - 1):
                    rs[i].append(rs[i - 1][j] + rs[i - 1][j + 1])
            rs[i].append(1)

        return rs
```

------------------------------------------------------
# 1332
题目：
> You are given a string s consisting only of letters 'a' and 'b'. In a single step you can remove one palindromic subsequence from s.
Return the minimum number of steps to make the given string empty.
A string is a subsequence of a given string if it is generated by deleting some characters of a given string without changing its order. Note that a subsequence does not necessarily need to be contiguous.
A string is called palindrome if is one that reads the same backward as well as forward.

```
Input: s = "baabb"
Output: 2
Explanation: "baabb" -> "b" -> "". 
Remove palindromic subsequence "baab" then "b".
```

神奇又毫无意义的理解题

代码：
```PYTHON
class Solution:
    def removePalindromeSub(self, s: str) -> int:
        if s == "":
            return 0
        if s == s[::-1]:
            return 1
        return 2
```


# 21 链表的合并
题目：
> You are given the heads of two sorted linked lists list1 and list2.
Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.
Return the head of the merged linked list.

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

注意好边界条件，就是当一个链表为空时要怎么处理。

代码：
```PYTHON
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        if list1.val < list2.val:
            rs = list1
        else:
            rs = list2

        while list1 or list2:
            if list1.val < list2.val:
                temp = list1.next
                list1.next = list2
                if not temp:
                    list1 = temp
                else:
                    break
            else:
                temp = list2.next
                list2.next = list1
                if not temp:
                    list2 = temp
                else:
                    break
                    
        return rs
```