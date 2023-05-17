---
title: Day5,6-leetcode
date: 2023-05-17 13:48:51
tags:
    - tree
    - graph
    - BFS
    - queue
    - stack
    - hash_table
    - Iterative method
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---
# 两日题目：263，1，226，119，367，349，405，225，617，350，1971，590
简单题都是基础题，基础也是学习的一个过程。图的题目都是相对较难的，要根据自己的理解去灵活处理图的题目。


# 263 丑数
题目：
> 丑数 就是只包含质因数 2、3 和 5 的正整数。
给你一个整数 n ，请你判断 n 是否为 丑数 。如果是，返回 true ；否则，返回 false 。
```
输入：n = 1
输出：true
解释：1 没有质因数，因此它的全部质因数是 {2, 3, 5} 的空集。习惯上将其视作第一个丑数。
```

代码：
```PYTHON
class Solution:
    def isUgly(self, n: int) -> bool:
        if n == 0:
            return False
        while True:
            if n == 1:
                return True
            if n % 2 == 0:
                n = n // 2
            elif n % 3 == 0:
                n = n // 3
            elif n % 5 == 0:
                n = n // 5
            else:
                return False
```

# 1 两数之和
题目：
> 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
你可以按任意顺序返回答案。
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

我采用的是替换掉第一个值来进行第二个数的判断。

代码：
```PYTHON
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i, num in enumerate(nums):
            nums[i] = None
            if target-num in nums:
                return [i, nums.index(target-num)]
            nums[i] = num
```

# 226 翻转二叉树
题目：
> 给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。
```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```
代码：
```PYTHON
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def reverse(node):
            if node is None:
                return
            node.left, node.right = node.right, node.left
            reverse(node.left)
            reverse(node.right)
        reverse(root)
        return root
```

# 119 杨辉三角
题目：
> 给定一个非负索引 rowIndex，返回「杨辉三角」的第 rowIndex 行。
在「杨辉三角」中，每个数是它左上方和右上方的数的和。
```
输入: rowIndex = 3
输出: [1,3,3,1]
```

用代码来模拟动态的相加过程，多用辅助值。

代码：
```PYTHON
class Solution:
    def getRow(self, rowIndex: int) -> List[int]:
        rs = [0]*(rowIndex+1)
        for i in range(rowIndex+1):
            rs[i] = 1
            temp = 1
            for j in range(1, i):
                temp1 = rs[j]
                rs[j] = temp + rs[j]
                temp = temp1
        return rs
```

# 367 有效的完全平方数
题目：
> 给你一个正整数 num 。如果 num 是一个完全平方数，则返回 true ，否则返回 false 。
完全平方数 是一个可以写成某个整数的平方的整数。换句话说，它可以写成某个整数和自身的乘积。
不能使用任何内置的库函数，如  sqrt 。
```
输入：num = 14
输出：false
解释：返回 false ，因为 3.742 * 3.742 = 14 但 3.742 不是一个整数。
```
继续使用牛顿迭代法求解。

斜率就是:
$$f'(x)=\frac{f(x)}{x_{n+1}-x_n}$$
转化为迭代式：
$$x_{n+1}=x_n-\frac{f(x_n)}{f'(x_n)}$$

将公式$f(x)=x^2-C$代入得到：
$$x_{n+1}=\frac{1}{2}(x_n+\frac{C}{x_n})$$

代码：
```PYTHON
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num == 1:
            return True
        n_0 = 1
        n_1 = 0.5 * (n_0 + num / n_0)
        while abs(n_1 - n_0) > 0.1:
            if int(n_1) ** 2 == num:
                return True
            n_0 = n_1
            n_1 = 0.5 * (n_0 + num / n_0)
        return False
```


# 349 交集
题目：
> 给定两个数组 nums1 和 nums2 ，返回 它们的交集 。输出结果中的每个元素一定是 唯一 的。我们可以 不考虑输出结果的顺序 。
```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

甚至用不到哈希表。

代码：
```PYTHON
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        rs = []
        for num in nums1:
            if num in nums2 and num not in rs:
                rs.append(num)
        return rs
```

# 405 16进制转换
题目：

十进制转换成二进制，要记住在位运算中可以直接使用二进制进行计算，所以这道题目最快速的做法就是使用二进制获取最后四位。
这里需要注意到的一个点是补码的规则，负数的补码就是最大值和它相加。（减去它的正数）

代码：
```PYTHON
class Solution:
    def toHex(self, num: int) -> str:

        if num == 0:
            return '0'

        hex_digits = "0123456789abcdef"
        mask = 0xf
        hex_str = ''

        if num < 0:
            num += 2 ** 32  

        while num != 0:
            hex_str = hex_digits[num & mask] + hex_str
            num >>= 4

        return hex_str
```

# 225
队列实现栈

记住队列要把最里面的用第二个存着才能访问到队列刚进去的那一个元素。

```python
from collections import deque
class MyStack:

    def __init__(self):
        self.queue1 = deque()
        self.queue2 = deque()


    def push(self, x: int) -> None:
        self.queue1.append(x)

    def pop(self) -> int:
        while len(self.queue1) > 1:
            self.queue2.append(self.queue1.popleft())
        element = self.queue1.popleft()
        self.queue1, self.queue2 = self.queue2, self.queue1
        
        return element


    def top(self) -> int:
        while len(self.queue1) > 1:
            self.queue2.append(self.queue1.popleft())
        element = self.queue1.popleft()
        self.queue1, self.queue2 = self.queue2, self.queue1
        self.queue1.append(element)

        return element


    def empty(self) -> bool:
        return len(self.queue1) == 0



# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
----------------------------------------------------------------------
# 617 合并二叉树
题目：
> 给你两棵二叉树： root1 和 root2 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

注意: 合并过程必须从两个树的根节点开始。
```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

随便一个遍历都可以。

代码：
```PYTHON
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        def preorderTraversal(node1, node2):
            if node1 is None and node2 is None:
                return None

            val1 = node1.val if node1 else 0
            val2 = node2.val if node2 else 0

            node3 = TreeNode(val1 + val2)

            node3.left = preorderTraversal(node1.left if node1 else None, node2.left if node2 else None)
            node3.right = preorderTraversal(node1.right if node1 else None, node2.right if node2 else None)

            return node3

        return preorderTraversal(root1, root2)
```

# 350 交集
题目：
> 给你两个整数数组 nums1 和 nums2 ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。
```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```

和昨天的题不一样，不想动脑筋直接替换值来避免重复计数，有个n^2复杂度也行。
想要快一些就用哈希表存值。

代码：
```PYTHON
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        hash_table = {}
        rs = []

        for num in nums1:
            hash_table[num] = hash_table.get(num, 0) + 1

        for num in nums2:
            if num in hash_table and hash_table[num] > 0:
                rs.append(num)
                hash_table[num] -= 1

        return rs
```

# 1971 存在路径问题
题目：
> 有一个具有 n 个顶点的 双向 图，其中每个顶点标记从 0 到 n - 1（包含 0 和 n - 1）。图中的边用一个二维整数数组 edges 表示，其中 edges[i] = [ui, vi] 表示顶点 ui 和顶点 vi 之间的双向边。 每个顶点对由 最多一条 边连接，并且没有顶点存在与自身相连的边。
请你确定是否存在从顶点 source 开始，到顶点 destination 结束的 有效路径 。
给你数组 edges 和整数 n、source 和 destination，如果从 source 到 destination 存在 有效路径 ，则返回 true，否则返回 false 。
```
输入：n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
输出：false
解释：不存在由顶点 0 到顶点 5 的路径.
```

最简单的广搜，思路就是创建一个图，然后用set()创建一个vis来判断有没有访问过。

代码：
```PYTHON
class Solution:
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        def dfs(i):
            if i == destination:
                return True
            vis.add(i)
            for j in g[i]:
                if j not in vis and dfs(j):
                    return True
            return False
            
        # 使用list作为默认工厂函数，将不存在的键的值初始化为空列表
        g = defaultdict(list)

        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        vis = set()

        return dfs(source)
```

# 387 哈希表
题目：
> 给定一个 n 叉树的根节点 root ，返回 其节点值的 后序遍历 。
n 叉树 在输入中按层序遍历进行序列化表示，每组子节点由空值 null 分隔（请参见示例）。
 
```
输入：root = [1,null,3,2,4,null,5,6]
输出：[5,6,3,2,4,1]
```

注意这是一个n叉树，它的children是一个列表。
其实也很简单就是它有孩子时访问所有孩子就好。

代码：
```PYTHON
class Solution:

    def postorder(self, root: 'Node') -> List[int]:
        rs = []
        def postorder_(node):
            if node is None:
                return
            if node.children is not None:
                for child in node.children:
                    postorder_(child)
            rs.append(node.val)
        postorder_(root)
        return rs
```


