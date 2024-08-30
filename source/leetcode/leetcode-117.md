---
title: 117. 填充每个节点的下一个右侧节点指针 II
date: 2023-11-03 11:20:06
tags:
  - 树
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/description/

<!-- more -->

> 给定一个二叉树：
>
> ```
> struct Node {
>   int val;
>   Node *left;
>   Node *right;
>   Node *next;
> }
> ```
>
> 填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL` 。
>
> 初始状态下，所有 next 指针都被设置为 `NULL` 。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)
>
> ```
> 输入：root = [1,2,3,4,5,null,7]
> 输出：[1,#,2,3,#,4,5,7,#]
> 解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连接），'#' 表示每层的末尾。
> ```
>
> **示例 2：**
>
> ```
> 输入：root = []
> 输出：[]
> ```
>
>  
>
> **提示：**
>
> - 树中的节点数在范围 `[0, 6000]` 内
> - `-100 <= Node.val <= 100`
>
> **进阶：**
>
> - 你只能使用常量级额外空间。
> - 使用递归解题也符合要求，本题中递归程序的隐式栈空间不计入额外空间复杂度。

### 题解一：DFS

使用数组记录每层的要连接的节点，数组下标表示层级，由于树的高度未知所以使用可变数组。

```java
class Solution {
    List<Node> depthHead = new ArrayList<>();

    public Node connect(Node root) {
        dfs(root, 0);
        return root;
    }

    public void dfs(Node node, int depth) {
        if (node == null) {
            return;
        }
        if (depth == depthHead.size()) {
            depthHead.add(node);
        } else {
            depthHead.get(depth).next = node;
            depthHead.set(depth, node);
        }
        dfs(node.left, depth + 1);
        dfs(node.right, depth + 1);
    }
}
```

### 题解二：BFS

由于是同深度间的连接，使用 BFS 具有天然的优势，只需将每次层级遍历的节点相连即可。

```java
class Solution {
    Queue<Node> queue = new ArrayDeque<>();

    public Node connect(Node root) {
        if (root == null) {
            return root;
        }
        queue.add(root);
        while (!queue.isEmpty()) {
            int n = queue.size();
            Node pre = null;
            for (int i = 0; i < n; i ++) {
                Node cur = queue.poll();
                if (pre != null) {
                    pre.next = cur;
                }
                pre = cur;
                if (cur.left != null) {
                    queue.add(cur.left);
                }
                if (cur.right != null) {
                    queue.add(cur.right);
                }
            }
        }
        return root;
    }
}
```

### 题解三：BFS + 链表

由于在 BFS 的过程中，我们实际上已经可以得到下一层的所有节点，那么我们可以直接对下一层进行连接，每一次 BFS 遍历的实际是已经连接好的链表。

```java
class Solution {
    public Node connect(Node root) {
        Node dummy = new Node();
        Node cur = root;
        while (cur != null) {
            dummy.next = null;
            Node nex = dummy;

            while (cur != null) {
                if (cur.left != null) {
                    nex.next = cur.left;
                    nex = cur.left;
                }
                if (cur.right != null) {
                    nex.next = cur.right;
                    nex = cur.right;
                }
                cur = cur.next;
            }
            cur = dummy.next;
        }
        return root;
    }
}
```

