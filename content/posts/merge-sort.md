---
title: 链表归并排序
date: 2023-06-05
categories:
- "coding"
tags:
- "链表"
- "排序"
featuredImagePreview: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230605150849270.png"
---

链表的归并排序算法

<!--more-->

归并排序主要就是分段+合并，可以自顶向下，也可以自底向上

## 合并

合并已经有基础：[合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

也没有什么花招，就是两边同时出发遍历，只是写起来相对麻烦

leetcode官方给的合并算法：

```c++
ListNode* merge(ListNode* head1, ListNode* head2) {
        ListNode* dummyHead = new ListNode(0);
        ListNode* temp = dummyHead, *temp1 = head1, *temp2 = head2;
        while (temp1 != nullptr && temp2 != nullptr) {
            if (temp1->val <= temp2->val) {
                temp->next = temp1;
                temp1 = temp1->next;
            } else {
                temp->next = temp2;
                temp2 = temp2->next;
            }
            temp = temp->next;
        }
        if (temp1 != nullptr) {
            temp->next = temp1;
        } else if (temp2 != nullptr) {
            temp->next = temp2;
        }
        return dummyHead->next;
    }
```

优点在于增加了一个虚拟头节点，使得统一性更好，不必单独判断第一个

## 自底向上方法

每两个合并一下，为了能够表示，使用`subLength`这个变量来划分

先取出来，排序好，再链回路

挺烦的

## 自顶向下方法

指定头尾，不断二分，递归的方法

找到中间节点可以用快慢指针

感觉还是递归简单一点