---
title: 24. 两两交换链表中的节点
date: 2023-07-12 15:05:52
tags:
  - 链表
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/swap-nodes-in-pairs/description/

<!-- more -->

> 给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/07/14/swap_ex1.jpg)
>
> ```
> 输入：head = [1,2,3,4]
> 输出：[2,1,4,3]
> ```
>
> **示例 2：**
>
> ```
> 输入：head = []
> 输出：[]
> ```
>
> **示例 3：**
>
> ```
> 输入：head = [1]
> 输出：[1]
> ```
>
>  
>
> **提示：**
>
> - 链表中节点的数目在范围 `[0, 100]` 内
> - `0 <= Node.val <= 100`

一看标签是一道做过的题，什么时候做过呢，再一看提交时间，一年以前。

对于链表两个节点的交换，在我大学时期 C 语言学习的过程中那是做过了诸多的练习，比如编写一个 `swap` 方法用来交换两个树，什么 `temp=a;a=b;b=temp` 简直就是烂熟于心。

## 解法一：迭代

构建一个 `dummyNode` 作为初始节点，然后判断后续两个节点是否为空，不为空则交换即可。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummyNode = new ListNode();
        dummyNode.next = head;
        ListNode curr = dummyNode;
        while (curr.next != null && curr.next.next != null) {
            ListNode left = curr.next;
            ListNode right = curr.next.next;
            left.next = right.next;
            curr.next = right;
            right.next = left;
            curr = curr.next.next;
        }
        return dummyNode.next;
    }
}
```

## 解法二：递归

既然是做重复的动作，那么使用递归就再合适不过了。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = head.next;
        head.next = swapPairs(newHead.next);
        newHead.next = head;
        return newHead;
    }
}
```

