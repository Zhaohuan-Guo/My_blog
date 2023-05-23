---
title: Day11-leetcode
date: 2023-05-23 23:59:39
tags:
    - tree
    - double pointer
    - stack
    - hash_table
categories: data_structure
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---
# 今日题目：20，17，88，557，965，599

# 20 括号的对应
题目：
> Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
An input string is valid if:
Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
```
Input: s = "()[]{}"
Output: true
```

栈的运用，运用栈来解决递归问题。

代码：
```PYTHON
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        bracket_map = {")": "(", "}": "{", "]": "["}
        for i in s:
            if i in '({[':
                stack.append(i)
            elif stack and bracket_map[i] == stack[-1]:
                stack.pop()
            else:
                return False
        return stack == []
```

# 17 回溯与递归
题目：
> Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.
A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```
这是一道回溯与递归的运用，其实在之前的二叉树遍历也是递归和回溯的使用。可以理解几个字母就是几个分支。注意好输出也就是边界条件

代码：
```PYTHON
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        map_ = {
            '2':['a','b','c'],
            '3':['d','e','f'],
            '4':['g','h','i'],
            '5':['j','k','l'],
            '6':['m','n','o'],
            '7':['p','q','r','s'],
            '8':['t','u','v'],
            '9':['w','x','y','z']
        }

        rs = []

        def backtrack(combination, digits, index):
            if index == len(digits):
                rs.append(combination)
                return
            
            curr_digit = digits[index]
            letters = map_[curr_digit]

            for letter in letters:
                backtrack(combination + letter, digits, index + 1)
        
        if digits:
            backtrack("", digits, 0)
        
        return rs
```

# 88 多指针
题目：
> You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.
Merge nums1 and nums2 into a single array sorted in non-decreasing order.
The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

最开始的思路也是维护三个指针，但是没有考虑到数组是升序的这个特点，所以出现了bug才转而用额外的空间来做这道题。

代码：
```PYTHON
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        # 初始化指针
        p1 = m - 1
        p2 = n - 1
        p = m + n - 1

        # 从后往前遍历数组，将较大的元素放入 nums1 的末尾
        while p1 >= 0 and p2 >= 0:
            if nums1[p1] > nums2[p2]:
                nums1[p] = nums1[p1]
                p1 -= 1
            else:
                nums1[p] = nums2[p2]
                p2 -= 1
            p -= 1

        # 如果 nums2 还有剩余元素，将其复制到 nums1 的前面
        nums1[:p2 + 1] = nums2[:p2 + 1]
```

# 557 字符串的拆分与拼接
题目：
> Given a string s, reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.
```
Input: s = "Let's take LeetCode contest"
Output: "s'teL ekat edoCteeL tsetnoc"
```

代码：
```PYTHON
class Solution:
    def reverseWords(self, s: str) -> str:
        s_list = s.split()
        rs_list = []
        for word in s_list:
            rs_list.append(word[::-1])
        rs = " ".join(rs_list)
        return rs

```

# 965 二叉树遍历
题目：
> A binary tree is uni-valued if every node in the tree has the same value.
Given the root of a binary tree, return true if the given tree is uni-valued, or false otherwise.
```
Input: root = [1,1,1,1,1,null,1]
Output: true
```
二叉树的遍历，没有什么难的，用列表的思路看待二叉树的题。要注意这里有一个变量要放到方法中去使用，用nonlocal。
这里介绍一下global和nonlocal的区别：
```global``` 关键字用于在函数内部访问和修改全局变量。当我们在函数内部需要修改全局变量的值时，需要使用 ```global``` 关键字声明变量为全局变量。这样，在函数内部对该变量的修改会影响到全局作用域中的变量。例如：
```python
count = 0

def increment():
    global count
    count += 1

increment()
print(count)  # 输出 1
```
```nonlocal``` 关键字用于在嵌套函数内部访问和修改**上一级**非全局作用域中的变量。当我们在一个嵌套函数内部需要修改上一级函数作用域中的变量时，需要使用 ```nonlocal``` 关键字声明变量为非局部变量。这样，在内层函数对该变量的修改会影响到上一级函数作用域中的变量。例如：
```python
def outer():
    count = 0

    def increment():
        nonlocal count
        count += 1

    increment()
    print(count)  # 输出 1

outer()

```


代码：
```PYTHON
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return False
        node = root
        value = node.val
        rs = 1
        def preorder(node):
            nonlocal rs
            if not node:
                return
            if node.val != value:
                rs = 0
                return
            preorder(node.left)
            preorder(node.right)
            
        preorder(root)
        return rs

```

# 599 哈希表
题目：
> Given two arrays of strings list1 and list2, find the common strings with the least index sum.
A common string is a string that appeared in both list1 and list2.
A common string with the least index sum is a common string such that if it appeared at list1[i] and list2[j] then i + j should be the minimum value among all the other common strings.
Return all the common strings with the least index sum. Return the answer in any order.
```
Input: list1 = ["Shogun","Tapioca Express","Burger King","KFC"], list2 = ["Piatti","The Grill at Torrey Pines","Hungry Hunter Steakhouse","Shogun"]
Output: ["Shogun"]
Explanation: The only common string is "Shogun".
```
用一个哈希表存第一个列表的坐标。
然后用一个辅助列表来保存坐标之和最小的word。

代码：
```PYTHON
class Solution:
    def findRestaurant(self, list1: List[str], list2: List[str]) -> List[str]:
        hash_table =  {}
        rs = []
        min_index = float('inf')
        for i, word in enumerate(list1):
            if word not in hash_table:
                hash_table[word] = i
        for i, word in enumerate(list2):
            if word in hash_table:
                sum_index = i + hash_table[word]
                if sum_index < min_index:
                    rs.clear()
                    rs.append(word)
                    min_index = sum_index
                elif sum_index == min_index:
                    rs.append(word)
        return rs

```