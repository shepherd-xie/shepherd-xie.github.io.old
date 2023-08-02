---
title: 143. 重排链表
date: 2023-07-31 14:56:48
tags:
  - 链表
  - 双指针
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/reorder-list/description/

<!-- more -->

> 给定一个单链表 `L` 的头节点 `head` ，单链表 `L` 表示为：
>
> ```
> L0 → L1 → … → Ln - 1 → Ln
> ```
>
> 请将其重新排列后变为：
>
> ```
> L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
> ```
>
> 不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/07/31/1626420311-PkUiGI-image.png)
>
> ```
> 输入：head = [1,2,3,4]
> 输出：[1,4,2,3]
> ```
>
> **示例 2：**
>
> ![img](https://images.orkva.com/images/2023/07/31/1626420320-YUiulT-image.png)
>
> ```
> 输入：head = [1,2,3,4,5]
> 输出：[1,5,2,4,3]
> ```
>
>  
>
> **提示：**
>
> - 链表的长度范围为 `[1, 5 * 104]`
> - `1 <= node.val <= 1000`

由于是单链表，所以处理起来稍微有些复杂，如果是双链表利用双指针一前一后处理即可。

对于单链表我们可以先从中点处阶段，再将后段部分链表反转，最后按规则拼接即可。

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }

    public void reorderList(ListNode head) {
        ListNode head2 = reverseList(middleNode(head));
        ListNode cur = head;
        ListNode cur2 = head2;
        while (cur2.next != null) {
            ListNode next = cur.next;
            ListNode next2 = cur2.next;
            cur.next = cur2;
            cur2.next = next;
            cur = next;
            cur2 = next2;
        }
    }
}
```

