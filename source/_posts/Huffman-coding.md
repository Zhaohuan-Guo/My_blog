---
title: Huffman coding
date: 2023-05-13 00:14:00
tags:
    - Huffman Coding
    - Lossless Compression
    - Encoding
categories: Algorithms
cover: https://store-images.s-microsoft.com/image/apps.29085.13916492596700196.f563a414-d055-4b56-9e94-9a27da6aa664.c648acf2-06f0-479e-982c-566e5044bbed
---

# 引入
假如我们有一段长度为10的字符串需要压缩，其中这个数字只由4个不同的数字组成。其中，a 有4个，b 有3个，c 有1个， d 有1个。
|Char|Times|
|------|-----|
|a|4|
|b|3|
|c|1|
|d|1|

对于原来的这个的字符串，一个字符就是一个字节，8个比特，那么需要80个比特来记录。

首先，现在我们进行初步的编码，使用00，01，10，11来表示四个字符，那么现在只需要20个比特来记录。
<table>
  <tr>
    <th rowspan="5">Code table</th>
    <th>Code word</th>
    <th>Char</th>
    <th>Times</th>
  </tr>
  <tr>
    <td>00</td>
    <td>a</td>
    <td>4</td>
  </tr>
  <tr>
    <td>01</td>
    <td>b</td>
    <td>3</td>
  </tr>
    <tr>
    <td>10</td>
    <td>c</td>
    <td>1</td>
  </tr>
    <tr>
    <td>11</td>
    <td>d</td>
    <td>1</td>
  </tr>
</table>
然后，继续改进，因为这个压缩后依然很大，当不同字符个数越多时，需要的比特位就越多。改进思路，出现次数越多用越少的比特位表示，我们将出现次数最多的a用1个比特表示，b用2个比特表示，c和d用3个比特表示。<br><br>
现在需要解决如何区分不同码字，引入一条编码规则：长的码字不能时短的码字的前缀。长的前面不能和短的重复。解码时就能从前往后查找，一旦找到匹配的就能确实是什么，且不会出错。这样压缩后的大小为：16比特。<br><br>

<table>
  <tr>
    <th rowspan="5">Code table</th>
    <th>Code word</th>
    <th>Char</th>
    <th>Times</th>
  </tr>
  <tr>
    <td>0</td>
    <td>a</td>
    <td>4</td>
  </tr>
  <tr>
    <td>10</td>
    <td>b</td>
    <td>3</td>
  </tr>
    <tr>
    <td>110</td>
    <td>c</td>
    <td>1</td>
  </tr>
    <tr>
    <td>111</td>
    <td>d</td>
    <td>1</td>
  </tr>
</table>

问题来了，如何找到这一堆符合规则的码字呢？答案：二叉树。
左边的枝是0，右边的枝是1。
```txt
               n
            /0   \1
           a      n
                /0  \1
               b     n
                   /0 \1
                  c     d               
```
# Huffman tree
在哈夫曼树中，每个字符对应一个叶子节点，而非叶子节点则表示字符的组合。树的构建过程中，根据字符的出现频率构建优先队列（最小堆），然后依次取出频率最低的两个节点，将它们合并为一个新的节点，并将新节点的频率设置为两个节点的频率之和。新节点成为新的最小频率节点，并重新插入到优先队列中，重复这个过程直到队列中只剩下一个节点，即根节点。

代码示例：
```python
from heapq import heappush, heappop
from typing import List

class TreeNode:
    def __init__(self, char: str, freq: int):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None

def build_huffman_tree(freq_list: List[int]) -> TreeNode:
    # 创建叶子节点-所有的节点都是叶子
    nodes = [TreeNode(chr(i + ord('a')), freq) for i, freq in enumerate(freq_list) if freq > 0]
    # 构建优先队列-就是小根堆
    heap = []
    for node in nodes:
        heappush(heap, (node.freq, id(node), node))

    # 构建哈夫曼树
    while len(heap) > 1:
        _, _, left = heappop(heap)
        _, _, right = heappop(heap)
        new_freq = left.freq + right.freq
        new_node = TreeNode('', new_freq)
        new_node.left = left
        new_node.right = right
        heappush(heap, (new_freq, id(new_node), new_node))

    _, _, root = heappop(heap)
    return root
```
构建完成后，可以根据哈夫曼树生成对应的编码表，将每个字符映射到对应的编码。编码的规则是：沿着哈夫曼树从根节点到叶子节点的路径，每经过左子节点记录一个'0'，每经过右子节点记录一个'1'。最终得到的编码表可以用于对数据进行压缩和解压缩。

# 案例
## 案例一：假设我们有一个字符串 "abracadabra"，我们想对它进行哈夫曼编码。
```python
freq_list = [5, 2, 1, 1, 2]  # 字符频率列表，对应字符'a'到'e'

root = build_huffman_tree(freq_list)

# 构建编码表
code_table = {}
def build_code_table(node, code):
    if node is None:
        return
    if node.char:
        code_table[node.char] = code
    build_code_table(node.left, code + '0')
    build_code_table(node.right, code + '1')

build_code_table(root, '')

# 打印编码结果
for char, code in code_table.items():
    print(f"{char}: {code}")

# 进行编码
encoded_string = ''.join(code_table[char] for char in "abeacadabea")
print("Encoded string:", encoded_string)

# 进行解码
decoded_string = ""
current_node = root
for bit in encoded_string:
    if bit == '0':
        current_node = current_node.left
    else:
        current_node = current_node.right
    if current_node.char:
        decoded_string += current_node.char
        current_node = root

print("Decoded string:", decoded_string)
'''
output:
a: 0
d: 100
c: 101
e: 110
b: 111
Encoded string: 01111100101010001111100
Decoded string: abeacadabea
'''
```
## 案例二：假设我们有一个文本文件 "input.txt"，我们想对它进行压缩和解压缩。
```python
# 读取文件内容
with open("input.txt", "r") as file:
    text = file.read()

# 统计字符频率
freq_list = [0] * 256  # 假设文件中的字符编码范围在0到255之间
for char in text:
    freq_list[ord(char)] += 1

# 构建哈夫曼树
root = build_huffman_tree(freq_list)

# 构建编码表
code_table = {}
build_code_table(root, '')

# 进行编码
encoded_string = ''.join(code_table[ord(char)] for char in text)

# 保存压缩结果
with open("compressed.bin", "wb") as file:
    byte_index = 0
    current_byte = 0
    for bit in encoded_string:
        current_byte = (current_byte << 1) | int(bit)
        byte_index += 1
        if byte_index == 8:
            file.write(bytes([current_byte]))
            byte_index = 0
            current_byte = 0
    if byte_index > 0:
        current_byte = current_byte << (8 - byte_index)
        file.write(bytes([current_byte]))

# 读取压缩文件
compressed_data = open("compressed.bin", "rb").read()

# 进行解压缩
decoded_string = ""
current_node = root
for byte in compressed_data:
    for i in range(7, -1, -1):
        bit = (byte >> i) & 1
        if bit == 0:
            current_node = current_node.left
        else:
            current_node = current_node.right
        if current_node.char:
            decoded_string += current_node.char
            current_node = root
```
在上述案例中，我们首先读取文件内容并统计字符频率，然后根据频率构建哈夫曼树。接下来，构建编码表并对文件内容进行编码。我们将编码后的结果写入一个二进制文件 "compressed.bin"。然后，我们读取压缩文件，并对其进行解压缩，最后将解压缩结果保存到 "output.txt" 文件中。