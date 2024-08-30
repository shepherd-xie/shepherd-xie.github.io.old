---
title: 144. 二叉树的前序遍历
date: 2023-08-17 10:07:47
tags:
  - 树
  - 二叉树
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/binary-tree-preorder-traversal/description/

<!-- more -->

> 给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/08/17/inorder_1.jpg)
>
> ```
> 输入：root = [1,null,2,3]
> 输出：[1,2,3]
> ```
>
> **示例 2：**
>
> ```
> 输入：root = []
> 输出：[]
> ```
>
> **示例 3：**
>
> ```
> 输入：root = [1]
> 输出：[1]
> ```
>
> **示例 4：**
>
> ![img](https://images.orkva.com/images/2023/08/17/inorder_5.jpg)
>
> ```
> 输入：root = [1,2]
> 输出：[1,2]
> ```
>
> **示例 5：**
>
> ![img](https://images.orkva.com/images/2023/08/17/inorder_4.jpg)
>
> ```
> 输入：root = [1,null,2]
> 输出：[1,2]
> ```
>
>  
>
> **提示：**
>
> - 树中节点数目在范围 `[0, 100]` 内
> - `-100 <= Node.val <= 100`
>
>  
>
> **进阶：**递归算法很简单，你可以通过迭代算法完成吗？

二叉树的先序遍历，有接触过二叉树的大多数都会写过。

### 递归

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        preorderTraversal(root, res);
        return res;
    }

    public void preorderTraversal(TreeNode root, List<Integer> res) {
        if (root != null) {
            res.add(root.val);
            preorderTraversal(root.left, res);
            preorderTraversal(root.right, res);
        }
    }
}
```

### 迭代

这里主要理一下迭代的思路。由于我们要记录所有未遍历的节点位置，且这些节点是具有先后关系的，所以可以用双端队列来实现。进一步思考由于是先序遍历，左子树一定是先进先出，所以用栈更合适。

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if (cur != null) {
                res.add(cur.val);
                stack.push(cur.right);
                stack.push(cur.left);
            }
        }
        return res;
    }
}
```

