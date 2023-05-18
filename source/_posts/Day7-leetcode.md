---
title: Day7-leetcode
date: 2023-05-18 16:18:48
tags:
    - tree
    - stack
    - hash_table
    - dynamic planning
categories: data_structure
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---
# 今日题目：144, 1342, 551, 83, 338, 509

今天的题目也没有偏难的题目，遍历的做法得总结，今天面试问到了层序遍历如何遍历。

# 263 前序遍历
题目：
> Given the root of a binary tree, return the preorder traversal of its nodes' values.
```
Input: root = [1,null,2,3]
Output: [1,2,3]
```
使用递归实现的前序遍历，需要去总结四种遍历方法的迭代写法。
这里使用栈来实现前序遍历，后入先出，后入的要是左节点，先入的是右节点。

代码：
```PYTHON
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return
        rs = []
        help_list = [root]
        while help_list:
            curr = help_list.pop()
            rs.append(curr.val)
            
            if curr.right:
                help_list.append(curr.right)
            if curr.left:
                help_list.append(curr.left)
        return rs

```

# 1342 Number of Steps to Reduce a Number to Zero

题目：
> Given an integer num, return the number of steps to reduce it to zero.
In one step, if the current number is even, you have to divide it by 2, otherwise, you have to subtract 1 from it.
```
Input: num = 14
Output: 6
Explanation: 
Step 1 14 is even; divide by 2 and obtain 7. 
Step 2 7 is odd; subtract 1 and obtain 6.
Step 3 6 is even; divide by 2 and obtain 3. 
Step 4 3 is odd; subtract 1 and obtain 2. 
Step 5 2 is even; divide by 2 and obtain 1. 
Step 6 1 is odd; subtract 1 and obtain 0.
```
十分简单的奇偶判断。

代码：
```PYTHON
class Solution:
    def numberOfSteps(self, num: int) -> int:
        rs = 0
        while num:
            if num % 2 == 0:
                num = num // 2
                rs += 1
            else:
                num -= 1
                rs += 1
        return rs
        

```


# 551 Student Attendance Record I


题目：
> You are given a string s representing an attendance record for a student where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:
'A': Absent.
'L': Late.
'P': Present.
The student is eligible for an attendance award if they meet both of the following criteria:
The student was absent ('A') for strictly fewer than 2 days total.
The student was never late ('L') for 3 or more consecutive days.
Return true if the student is eligible for an attendance award, or false otherwise.
```
Input: s = "PPALLL"
Output: false
Explanation: The student was late 3 consecutive days in the last 3 days, so is not eligible for the award.
```

只用考虑在尾部时候角标溢出的情况，解决办法就是创建一个更大的字符串来进行判断。

代码：
```PYTHON
class Solution:
    def checkRecord(self, s: str) -> bool:
        count = 0
        s_n = s + 'PP'
        for i, char in enumerate(s):
            if char == 'A':
                count += 1
                if count > 1:
                    return False
            elif char == 'L' and s_n[i+1] == 'L' and s_n[i+2] == 'L':
                return False
        return True

```

# 83. Remove Duplicates from Sorted List

题目：
> Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.
```
Input: head = [1,1,2]
Output: [1,2]
```
在列表的基础上多加上一个指针跟着去操作节点。

代码：
```PYTHON
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        hash_table = {}
        node = head
        pre = None
        while node:
            if node.val not in hash_table:
                hash_table[node.val] = 1
                pre = node
                node = node.next
            else:
                pre.next = node.next
                node = node.next
        return head

```
-----------------------------------------------------------------
# 338 动态规划
题目：
> Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.
```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```

动态规划的题，重要的是要弄清楚**动态变化的规律**，究竟用到了之前的哪些值得弄明白。

代码：
```PYTHON
class Solution:
    def countBits(self, n: int) -> List[int]:
        rs = [0]*(n+1)
        for i in range(n+1):
            if i % 2 == 0:
                rs[i] = rs[i >> 1]
            else:
                rs[i] = rs[i >> 1] + 1
        return rs
```
-----------------------------------------------------------------
# 509

斐波那契数列

代码：
```PYTHON
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n
        a = 0
        b = 1
        for i in range(n-1):
            temp = b
            b = a + b
            a = temp
        return b

```