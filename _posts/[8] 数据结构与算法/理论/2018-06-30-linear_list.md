---
layout: post
title: 【算法】线性表
categories:
tags: 0x80_数据结构与算法
keywords:
description:
order: 501
---


主要内容
- 顺序表
- 链表
  - 单链表
  - 单循环链表
  - 双向循环链表
- 跳跃表
- 并查集






## 1. 顺序表
顺序表是在内存中连续存放的数组。  
顺序表有如下操作：
- 初始化
- 求元素个数
- 在i位置插入一个元素。从后往前，依次后移1格，直到i位置。
- 删除一个元素。类似的相反操作


因此，顺序表的增、删操作，时间复杂度都是 O(n)


## 2. 单链表
有两种：带头结点单链表（用一个空节点作为头部结点），不带头结点单链表（用第一个数据节点作为头部结点）  
不带头结点单链表对第一个元素增、删时，与其它元素的增删操作不一致，所以一般使用带头结点  

带头节点的单链表
```py
class Node(object):
    def __init__(self, val):
        self.val = val
        self.next = None

    def __repr__(self):
        return str(self.val)


# 改为带头的
class MyLinkedList:
    def __init__(self):
        self.head = Node('□')
        self.size = 0

    def get(self, index):
        if index < 0 or index >= self.size:
            raise IndexError('index out of range')
        curr = self.head
        for i in range(index + 1):
            curr = curr.next
        return curr.val

    def add_at_head(self, val):
        node_tmp = Node(val)
        node_tmp.next = self.head.next
        self.size += 1

    def add_at_tail(self, val):
        curr = self.head
        while curr.next is not None:
            curr = curr.next
        curr.next = Node(val)
        self.size += 1

    def add_at_index(self, index, val):
        if index < 0 or index > self.size:
            raise IndexError('index out of range')
        else:
            node_new = Node(val)
            curr = self.head
            for i in range(index):
                curr = curr.next
            node_new.next = curr.next
            curr.next = node_new
            self.size += 1

    def delete_at_index(self, index):
        if index < 0 or index >= self.size:
            raise IndexError('index out of range')
        curr = self.head
        for i in range(index):
            curr = curr.next
        curr.next = curr.next.next

        self.size -= 1

    def __repr__(self):
        curr = self.head
        list_str = curr.val
        curr = curr.next
        while curr is not None:
            list_str += ' -> ' + str(curr.val)
            curr = curr.next
        return list_str
```




不带头节点的单链表，增删操作时需要对头节点特殊处理，代码量略多一些，LC用的这个格式，所以也写出来
```py
class Node(object):
    def __init__(self, val):
        self.val = val
        self.next = None

    def __repr__(self):
        return str(self.val)


class MyLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0

    def get(self, index):
        if index < 0 or index >= self.size:
            raise IndexError('index out of range')
        curr = self.head
        for i in range(index):
            curr = curr.next
        return curr.val

    def add_at_head(self, val):
        head_new = Node(val)
        head_new.next = self.head
        self.head = head_new
        self.size += 1

    def add_at_tail(self, val):
        curr = self.head
        if curr is None:
            self.head = Node(val)
        else:
            while curr.next is not None:
                curr = curr.next
            curr.next = Node(val)
        self.size += 1

    def add_at_index(self, index, val):
        if index < 0 or index > self.size:
            raise IndexError('index out of range')
        elif index == 0:
            self.add_at_head(val)
        else:
            node_new = Node(val)
            curr = self.head
            for i in range(index - 1):
                curr = curr.next
            node_new.next = curr.next
            curr.next = node_new
            self.size += 1

    def delete_at_index(self, index):
        if index < 0 or index >= self.size:
            raise IndexError('index out of range')
        curr = self.head
        if index == 0:
            self.head = curr.next
            return
        for i in range(index - 1):
            curr = curr.next
        curr.next = curr.next.next

        self.size -= 1

    def __repr__(self):
        list_str = '□'
        curr = self.head
        while curr is not None:
            list_str += ' -> ' + str(curr.val)
            curr = curr.next
        return list_str
```






### Two Pointer Technique
[Two Pointer Technique](https://leetcode.com/explore/learn/card/linked-list/214/two-pointer-technique/)  
1. Two pointers starts at different position: one starts at the beginning while another starts at the end;
2. Two pointers are moved at different speed: one is faster while another one might be slower.


一个来自 LeetCode的案例 [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/)
```py
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if head is None:
            return False
        fast=head
        slow=head
        while True:
            if (fast is None) or (slow is None):
                return False
            if (fast.next is None) or (fast.next.next is None) or (slow.next is None):
                return False
            fast=fast.next.next
            slow=slow.next
            if fast is slow:
                return True
```

### 反转单链表
```py
def reverseList(self, head):
    """
    :type head: ListNode
    :rtype: ListNode
    """
    if head is None:
        return None
    curr=head
    while curr.next:
        tmp=curr.next
        curr.next=curr.next.next
        tmp.next=head
        head=tmp
    return head
```

## 环形链表

其实现与单链表很相似，不过检查结束的条件是 `curr.next == head`

## 双向链表


## 跳跃表

[参考](https://mp.weixin.qq.com/s/AGPCfFg7bEiCsa5zNeCi4A)


为什么：
- 顺序表。查找如果可以用二分法，复杂度是  O(logn), 插入和删除都是 O(n)
- 链表不能用二分法，查找复杂度是O(n),插入、删除复杂度是 O(1)
- 二叉树，虽然插入、删除、查找也是 O(logn)，但仅限于平衡二叉树，遇到严重偏到一边的二叉树，复杂度仍然是 O(n)
- 红黑树。本身实现很复杂，并且插入、删除时，同时做一次平衡，提高了一定的花销。

跳跃表是什么？


![catenary1](/pictures_for_blog/algorithm/skip_list.jpg)

- 查找：这就可以用二分法了，复杂度 O(logn)
- 插入：**抛硬币来决定新插入结点跨越的层数**：每次我们要插入一个结点的时候，就来抛硬币，如果抛出来的是 **正面**，则继续抛，直到出现 **负面** 为止，统计这个过程中出现正面的 **次数**，这个次数作为结点跨越的层数。
- 删除：从每个链条删除即可

查找、插入、删除复杂度都是 O(logn)

总结下跳跃表的有关性质：
1. 跳跃表的每一层都是一条有序的链表.
2. 跳跃表的查找次数近似于层数，时间复杂度为O(logn)，插入、删除也为 O(logn)。
3. 最底层的链表包含所有元素。
4. 跳跃表是一种随机化的数据结构(通过抛硬币来决定层数)。
5. 跳跃表的空间复杂度为 O(n)。
