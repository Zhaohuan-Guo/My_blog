---
title: Day12-leetcode
date: 2023-05-26 21:52:51
tags:
---
今天改变复习策略。
对于leetcode的刷题要侧重于将python代码转化为js

今日题目：3，206
# 3. 无重复字符的最长子串
给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

解题思路使用队列复杂度（n）。最初思路是使用动态指针，这样做最坏复杂度会有（2n）。

python代码：
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0
        i = 0
        queue = deque([])
        rs = 0
        l = len(s)
        while i < l:
            while s[i] in queue:
                rs = max(len(queue), rs)
                queue.popleft()
            queue.append(s[i])
            i += 1
        rs = max(len(queue), rs)
        return rs
```

JS代码：
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    if (!s) {
        return 0;
    }
    let i = 0;
    let queue = [];
    let rs = 0;
    let l = s.length;
    while (i<l){
        while(queue.includes(s[i])){
            rs = Math.max(queue.length, rs);
            queue.shift();
        }
        queue.push(s[i]);
        i++;
    }
    rs = Math.max(queue.length, rs);
    return rs;

};
```
# 206 翻转链表
easy
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    let pre = null;
    let curr = head;
    let next;
    while(curr !== null){
        next = curr.next 
        curr.next = pre
        pre = curr
        curr = next
    }
    return pre
};
```
学习一个新的做法：
```javascript

/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
  let newHead = null;

  if (!head){
      return null
  }

  const traverse = (pre, current) => {
    if (current?.next) {
      traverse(current, current.next);
    } else {
      newHead = current;
    }

    current && (current.next = pre);
  };

  traverse(null, head);

  return newHead;
};
```

这个解答中用了递归，实现了逆序遍历整个链表，这种方法是可以借鉴的。
