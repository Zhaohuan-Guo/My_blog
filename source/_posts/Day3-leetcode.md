---
title: Day3-leetcode
date: 2023-05-14 22:40:38
tags: 
    - data_structure
    - hash table
    - linked list
    - tree
categories: data_structure
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---
# 今日题目：219, 27, 110, 594, 1668, 206
今天的题目看似简单，但实际上需要考虑很多细节，当没有在最开始想到最简单解决办法时就得考虑各种边界条件了。
# 219 
题目：hash_table
> Given an integer array nums and an integer **k**, return true if there are two distinct indices **i** and **j** in the array such that **nums[i] == nums[j]** and **abs(i - j) <= k**.
```example
Input: nums = [1,2,3,1], k = 3
Output: true
```
很多方法可以做这个题，最简单的就是用哈希表，边遍历，边插入表，边查询时候满足条件。

代码：
```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        hash_table = {}
        for i, num in enumerate(nums):
            if num in hash_table and i - hash_table[num] <= k:
                return True
            hash_table[num] = i
        return False
```
# 27
题目：
> Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.
Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:
Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.
```example
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).

```
理解好题目之后就是简单的遍历一遍就好

代码：
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        count = 0
        for i, num in enumerate(nums):
            if num != val:
                nums[count] = num
                count += 1
        return count
```
# 110 Tree
题目：
> Given a binary tree, determine if it is height-balanced.
Height-Balanced：
A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

```example
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```
这题时一个双重递归，先是要递归得到每一个节点的高度，然后再递归判断是不是平衡树，只要有不满足的直接return false。

在解决递归问题时，要手动实现一般的循环，然后考虑边界条件后再敲代码。

示例代码求高度时冗余了：可以用三目表达式：
```python 
return 0 if not node else max(get_height(node.left), get_height(node.right)) + 1
```

代码：
```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if root is None:
            return True

        def get_height(node):
            if node is None:
                return 0
            left_height = 0 if not node.left else get_height(node.left)
            right_height = 0 if not node.right else get_height(node.right)
            return max(left_height, right_height) + 1

        if self.isBalanced(root.left) and self.isBalanced(root.right):
            if abs(get_height(root.left) - get_height(root.right)) > 1:
                return False
            else:
                return True
```
# 594 hash_table
题目：
> We define a harmonious array as an array where the difference between its maximum value and its minimum value is exactly 1.
Given an integer array nums, return the length of its longest harmonious subsequence among all its possible subsequences.
A subsequence of array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements.
```example
Input: nums = [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
```
这个题也是用哈希表，哈希表有两种创建用的方法，第一种直接用列表，这样自然地升序，用这个来排序的时间复杂度就为O(1)最快的排序方法，但是占空间。第二种用字典，当然字典也可以升序，用```sorted_keys = sorted(hash_table.keys())```就可以升序进行后面的操作了。

代码：
```python
class Solution:
    def findLHS(self, nums: List[int]) -> int:
        rs = 0
        hash_table = {}
        for num in nums:
            if num in hash_table:
                hash_table[num] += 1
            else:
                hash_table[num] = 1

        sorted_keys = sorted(hash_table.keys())
        for key in sorted_keys:
            if key+1 in hash_table:
                rs = max(rs, hash_table[key]+hash_table[key+1])

        return rs
```
# 1668 字符串基本操作
题目：
> For a string sequence, a string word is k-repeating if word concatenated k times is a substring of sequence. The word's maximum k-repeating value is the highest value k where word is k-repeating in sequence. If word is not a substring of sequence, word's maximum k-repeating value is 0.
Given strings sequence and word, return the maximum k-repeating value of word in sequence.

```example
Input: sequence = "ababc", word = "ba"
Output: 1
Explanation: "ba" is a substring in "ababc". "baba" is not a substring in "ababc".

```
这个题目有个误区，不能直接用replace进行替换然后再数数，

代码：
```python
class Solution:
    def maxRepeating(self, sequence: str, word: str) -> int:
        rs = 0
        while word*(1+rs) in sequence:
            rs += 1
        return rs

```
# 206 链表
反转链表，要记得斩断之前要保存好下一个节点，然后就是变量名的转化。

代码：
```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre_node = None
        curr_node = head
        while curr_node:
            next_node = curr_node.next
            curr_node.next = pre_node
            pre_node = curr_node
            curr_node = next_node
        return pre_node
```