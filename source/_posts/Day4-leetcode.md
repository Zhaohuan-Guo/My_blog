---
title: Day4-leetcode
date: 2023-05-15 16:10:12
tags: 
    - data_structure
    - hash table
    - linked list
    - tree
categories: data_structure
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---
# 今日题目：387， 700， 645，232，70，26

今天题目都是五分钟的小题，不做解释。

# 387 哈希表
题目：
> Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.
```
Input: s = "loveleetcode"
Output: 2
```
代码：
```PYTHON
class Solution:
    def firstUniqChar(self, s: str) -> int:
        hash_table = {}
        for i in s:
            if i not in hash_table:
                hash_table[i] = 1
            else:
                hash_table[i] += 1
        for i, char in enumerate(s):
            if hash_table[char] == 1:
                return i
        return -1
```
# 387 哈希表
题目：
> Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.
```
Input: s = "loveleetcode"
Output: 2
```
代码：
```PYTHON
class Solution:
    def firstUniqChar(self, s: str) -> int:
        hash_table = {}
        for i in s:
            if i not in hash_table:
                hash_table[i] = 1
            else:
                hash_table[i] += 1
        for i, char in enumerate(s):
            if hash_table[char] == 1:
                return i
        return -1
```

# 700 树的遍历
题目：
> You are given the root of a binary search tree (BST) and an integer val.
Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.

```
Input: root = [4,2,7,1,3], val = 2
Output: [2,1,3]
```
代码：
```PYTHON
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        curr = root
        help_list = []
        while curr or help_list:
            if curr:
                help_list.append(curr)
                curr = curr.left
            else:
                curr = help_list.pop()
                if curr.val == val:
                    return curr
                curr = curr.right
```
# 645 哈希表
题目：
> You have a set of integers s, which originally contains all the numbers from 1 to n. Unfortunately, due to some error, one of the numbers in s got duplicated to another number in the set, which results in repetition of one number and loss of another number.
You are given an integer array nums representing the data status of this set after the error.
Find the number that occurs twice and the number that is missing and return them in the form of an array.

```
Input: nums = [1,2,2,4]
Output: [2,3]
```
代码：
```PYTHON
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        hash_table = [1]+[0]*len(nums)
        rs = [0, 0]
        for num in nums:
            hash_table[num] += 1
        for key, val in enumerate(hash_table):
            if val == 2:
                rs[0] = key
            elif val == 0:
                rs[1] = key
        return rs
```

# 232 队列
题目：
创建一个队列。
代码：
```PYTHON
class MyQueue:

    def __init__(self):
        self.stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)

    def pop(self) -> int:
        return self.stack.pop(0)

    def peek(self) -> int:
        return self.stack[0]

    def empty(self) -> bool:
        if len(self.stack) == 0:
            return True
        return False
```
# 70 斐波那契数列
题目：
就是一般的斐波那契数列。

代码：
```PYTHON
class Solution:
    def climbStairs(self, n: int) -> int:
        a = 0
        b = 1
        rs = 0
        for i in range(n):
            rs = a + b
            a = b
            b = rs

        return rs
```
# 26 哈希表
题目：
> Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.
Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:
Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.

```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

```
和昨天哈希表的题差不多。
代码：
```PYTHON
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        hash_table = {}
        count = 0
        for num in nums:
            if num not in hash_table:
                hash_table[num] = 1
                nums[count] = num
                count += 1
        return count
```