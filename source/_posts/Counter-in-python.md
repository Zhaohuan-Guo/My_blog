---
title: Counter in python
date: 2023-05-22 22:52:18
tags:
    - python
categories: python
cover: https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Python-logo-notext.svg/1869px-Python-logo-notext.svg.png
---
# 计数器的使用
计数器对象（Counter）是 Python 标准库 collections 模块中的一个类，用于方便地进行计数操作。它提供了一种简单而高效的方式来统计可哈希对象（如字符串、列表、元组等）中元素的出现次数。
## 创建计数器对象
```python
from collections import Counter

counter = Counter()  # 创建一个空的计数器对象
counter = Counter(iterable)  # 使用可迭代对象创建计数器对象
```

## 计数元素
```python
counter.update(iterable)
```

## 获取计数值
```python
count = counter[element]  # 获取元素的计数值
```

## 计数器对象的操作：
- 合并计数器对象：使用 + 运算符可以合并两个计数器对象。
- 获取最常见的元素：使用 ```most_common()``` 方法可以按照计数值降序返回计数器中最常见的元素及其计数值

示例
```python
from collections import Counter

# 创建计数器对象并计数
words = ["apple", "banana", "apple", "orange", "banana", "apple"]
counter = Counter(words)

# 获取元素的计数值
print(counter["apple"])  # 输出: 3
print(counter["banana"])  # 输出: 2
print(counter["orange"])  # 输出: 1

# 合并计数器对象
other_words = ["apple", "pear", "pear"]
other_counter = Counter(other_words)
merged_counter = counter + other_counter
print(merged_counter)  # 输出: Counter({'apple': 4, 'banana': 2, 'pear': 2, 'orange': 1})

# 获取最常见的元素
most_common = merged_counter.most_common(2)
print(most_common)  # 输出: [('apple', 4), ('banana', 2)]
```