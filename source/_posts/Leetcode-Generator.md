---
title: Leetcode_Generator
date: 2023-05-12 00:52:38
tags: 
    - python 
    - daily
categories: daily_blog
cover: https://repository-images.githubusercontent.com/314091861/8c489d80-2ad2-11eb-96f0-9559eae37f57
---


# 刷题介绍
## 整体介绍
    - 将收集到的三百多道题目以随机的方式抽取，并且先抽取完简单题再抽取中等题。
    - 保证即使刷到150道也能涵盖大致知识点，保证广度的同时加深深度。
    - 简单题抽取6道，中等题4道
    - 做题时间（开始做到把题目弄懂）为2~4小时
    - 其他基础知识等时间充裕再做整理
    - github链接为：https://github.com/Zhaohuan-Guo/leetcode_questions
    - 要看抽过的题：github里面有个questionList可以查看，也可以之间点开那个past_questions.txt看
## 题目涵盖
题库信息如下：
```txt
quit(q), insert(i), info, get_question(gq), past_questions(pq), check_today_questions(c)
info
            total question number: 369
            easy question number: 96
            medium question number: 273
            done questions number: 6
            rest questions number: 363
            rest easy question number: 90
            rest medium question number: 273
```
题目涵盖了以下内容
1. 数学基础
    1. 位运算
    2. 其他
2. 数据结构
    1. 线性表
        1. 数组
        2. 链表
        3. 栈
        4. 队列
        5. 字符串
        6. 哈希表
    2. 树
        1. 二叉树
            1. 普通二叉树
            2. 二叉搜索树
            3. 平衡二叉树
        2. 多叉树
        3. 并查集
        4. 字典树
    3. 图
        1. BFS
        2. DFS
        3. 拓扑排序
        4. 二分图
        5. 最小生成树
        6. 最短路径
        7. 图的遍历
    4. 数据结构设计
3. 基础算法/思想
    1. 排序
    2. 二分搜索
    3. 递归
    4. 回溯算法
    5. 动态规划
        1. 一维动态规划
        2. 多维动态规划
    6. 贪心
4. 其它
    1. 滑动窗口
    2. 前缀和
    3. 差分数组
    4. 区间问题
    5. Boyer-Moore 投票算法
    6. 洗牌算法
# 代码
``` python
'''
enter "quit", then quit
enter "insert", then enter question_idx and level
    enter "quit", quit to main
enter "info", show the information of the question database
enter "get_question", show 4 questions of today, and delete the questions
from database
enter "past_questions", print the questions_idx and time of past_question
'''
import random
import datetime
import pytz

data_list = []
easy_list = []
medium_list = []

total_num = 0
easy_num = 0
medium_num = 0

data_file = "data.txt"
easy_file = "easy.txt"
medium_file = "medium.txt"
past_que_file = "past_questions.txt"

def get_time():
    tz = pytz.timezone('Europe/Dublin')
    ireland_time = datetime.datetime.now(tz)
    ireland_date = ireland_time.date()
    return str(ireland_date)

def write_to_file(file_name, data, type):
    with open(file_name, type) as f:
        f.write(str(data) + '\n')

def read_file(file_name):
    with open(file_name, 'r') as f:
        lines = f.readlines()
        return [line.strip() for line in lines if line != '\n']

def get_questions(file_name):
    temp_arr = []
    with open(past_que_file, 'r') as f:
        lines = f.readlines()
        for line in lines:
            if line != '\n' and line.split()[0] == get_time():
                print("today's questions has got, please check_today_questions")
                return
    with open(file_name, 'r') as f:
        lines = f.readlines()
        temp_arr += [int(line.strip()) for line in lines]
        select_numbers = random.sample(temp_arr, 6)
        print("today question:", select_numbers)
        for _ in select_numbers:
            write_to_file(past_que_file, get_time() + "  " + str(_), 'a')
        temp_str = ''
        for x in temp_arr:
            if x not in select_numbers:
                temp_str += str(x) + '\n'
        write_to_file(file_name, temp_str, 'w')

while True:
    get_time()
    print("\nquit(q), insert(i), info, get_question(gq), past_questions(pq), check_today_questions(c)")
    ipt = input()
    if ipt == "quit" or ipt == "q":
        print('program stop')
        break
    if ipt == "insert" or ipt == "i":
        while True:
            ipt = input()
            if ipt == "quit" or ipt == "q":
                break
            que_num, level = map(int, ipt.split())
            data_list.append((que_num, level))
            write_to_file(data_file, ipt, 'a')
            if level == 1:
                easy_list.append(que_num)
                write_to_file(easy_file, que_num, 'a')
            elif level == 2:
                medium_list.append(que_num)
                write_to_file(medium_file, que_num, 'a')
    if ipt == "info":
        data = read_file(data_file)
        total_num = len(data)
        for arr in data:
            temp_arr = arr.split()
            if temp_arr[1] == '1':
                easy_num += 1
            else:
                medium_num += 1
        pt = f'''        total question number: {total_num}
        easy question number: {easy_num}
        medium question number: {medium_num}
        done questions number: {len(read_file(past_que_file))}
        rest questions number: {total_num - len(read_file(past_que_file))}
        rest easy question number: {len(read_file(easy_file))}
        rest medium question number: {len(read_file(medium_file))}'''
        print(pt)
    if ipt == "get_question" or ipt == "gq":
        if len(read_file(easy_file)) == 0:
            get_questions(medium_file)
        else:
            get_questions(easy_file)
    if ipt == "past_questions" or ipt == "pq":
        temp_arr = read_file(past_que_file)
        for arr in temp_arr:
            print(arr)
    if ipt == "check_today_questions" or ipt == 'c':
        temp_arr = read_file(past_que_file)
        for arr in temp_arr:
            if arr != '' and arr.split()[0] == get_time():
                print(arr)
```
# 参考
1. [CSDN博客：刷题目录](https://blog.csdn.net/weixin_43004044/article/details/122477673)
2. [LeetCode官网](https://leetcode.com/)
3. [我的github](https://github.com/Zhaohuan-Guo/leetcode_questions)