---
title: Day2-leetcode
date: 2023-05-13 16:15:44
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
mathjax: true
---
# 今日题目：94， 69， 530， 237， 292，415
今天题目除了415都是很简单的题目，415麻烦在很多边界条件的考虑容易将题目复杂化。
# 94 InorderTraversal
题目：
> Given the root of a binary tree, return the inorder traversal of its nodes' values.
```txt
Input: root = [1,null,2,3]
Output: [1,3,2]
```

中序遍历没有什么难的，可以尝试用迭代来解决，迭代的思路和递归完全不一样
代码：
```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        node = root
        rs_list = []
        help_list = []

        while node or help_list:
            while node:
                help_list.append(node)
                node = node.left
            node = help_list.pop()
            rs_list.append(node.val)
            node = node.right

        return rs_list
```

# 69 square root
题目：
> Given a non-negative integer **x**, return the square root of **x** rounded down to the nearest integer. The returned integer should be non-negative as well.
You must not use any built-in exponent function or operator.
For example, do not use **pow(x, 0.5)** in c++ or **x ** 0.5** in python.
```txt
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```
从头遍历一个个都能解答，想快一点就二分，想再快就牛顿迭代法，这里介绍牛顿迭代法。
迭代法是从初始点开始进行一系列计算毕竟目标值。牛顿迭代法是在图像里找到初始点 $x_n$，画一条切线，切线和x轴的交点作为 $ x_{n+1} $。然后不断迭代。

斜率就是:
$$f'(x)=\frac{f(x)}{x_{n+1}-x_n}$$
转化为迭代式：
$$x_{n+1}=x_n-\frac{f(x_n)}{f'(x_n)}$$

将公式$f(x)=x^2-C$代入得到：
$$x_{n+1}=\frac{1}{2}(x_n+\frac{C}{x_n})$$

代码：
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x == 0:
            return 0
        x_n = x
        while x_n ** 2 - x > 0.1:
            x_n = (x_n + x / x_n) / 2

        return int(x_n)
```
# 530-InorderTraversal
题目：
> Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.
```
Input: root = [4,2,6,1,3]
Output: 1
```
又是中序遍历

代码：
```python
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> float:
        min_val = float('inf')
        curr_val = None
        def inorderTraversal(node):
            nonlocal curr_val, min_val
            if node is None:
                return
            inorderTraversal(node.left)
            if curr_val is not None and curr_val !=node.val:
                diff_val = node.val - curr_val
                curr_val = node.val
                min_val = min(min_val, diff_val)
            elif curr_val is None:
                curr_val = node.val
            inorderTraversal(node.right)
        inorderTraversal(root)

        return min_val
```
# 237
链表的删除：指向下一个节点就好了。
链表的题都是对节点进行处理就好了，不用管输出。
```python
class Solution:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val = node.next.val
        node.next = node.next.next
```

# 292 归纳题
题目：
> You are playing the following Nim Game with your friend:
Initially, there is a heap of stones on the table.
You and your friend will alternate taking turns, and you go first.
On each turn, the person whose turn it is will remove 1 to 3 stones from the heap.
The one who removes the last stone is the winner.
Given n, the number of stones in the heap, return true if you can win the game assuming both you and your friend play optimally, otherwise return false.
```
Input: n = 4
Output: false
Explanation: These are the possible outcomes:
1. You remove 1 stone. Your friend removes 3 stones, including the last stone. Your friend wins.
2. You remove 2 stones. Your friend removes 2 stones, including the last stone. Your friend wins.
3. You remove 3 stones. Your friend removes the last stone. Your friend wins.
In all outcomes, your friend wins.
```
数学题，找规律，以后碰到类似的题都可以按顺序去归纳。

代码：
```python
return !(x%4 == 0)
```

# 415 双指针
题目：
> Given two non-negative integers, num1 and num2 represented as string, return the sum of num1 and num2 as a string.
You must solve the problem without using any built-in library for handling large integers (such as BigInteger). **You must also not convert the inputs to integers directly.**
```
Input: num1 = "456", num2 = "77"
Output: "533"
```
代码优化：

你的解决方案思路是正确的，但是有一些地方可以进行优化和改进。下面是对你的代码进行评价和优化的建议：

使用双指针替代列表的弹出操作：你使用两个列表 num1_list 和 num2_list 来保存输入字符串中的字符，并使用 pop() 方法逐个弹出字符进行计算。这种做法在时间复杂度上是 O(n)，其中 n 是输入字符串的长度。可以通过使用双指针来遍历字符串，而不必将其转换为列表，从而减少空间和时间的开销。

简化进位标记和字符计算：你使用了 carry 标志来表示进位，并通过字符的 ASCII 值进行计算。这种方法会增加代码的复杂性。可以考虑使用整数运算来进行字符转换和进位的处理，简化逻辑并提高执行效率。

使用列表反转代替字符串拼接：你将结果字符逐个追加到列表 rs_list 中，然后使用 rs += rs_list.pop() 将字符拼接成字符串。这种方法在每次追加字符时需要进行列表操作，而列表的 pop() 方法是一个相对较慢的操作。可以使用列表的 append() 方法将字符追加到列表末尾，然后在最后一步直接使用列表的反转操作，即 rs_list[::-1]，将列表中的字符反转顺序，得到最终的结果字符串。

代码：
```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        i = len(num1) - 1
        j = len(num2) - 1
        carry = 0
        rs_list = []

        while i >= 0 or j >= 0 or carry > 0:
            n1 = int(num1[i]) if i >= 0 else 0
            n2 = int(num2[j]) if j >= 0 else 0
            current_sum = n1 + n2 + carry
            carry = current_sum // 10
            digit = current_sum % 10

            rs_list.append(str(digit))

            i -= 1
            j -= 1

        return ''.join(rs_list[::-1])
```