---
title: 94. 二叉树的中序遍历
date: 2023-08-18 13:01:24
tags:
  - 树
  - 二叉树
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/binary-tree-inorder-traversal/description/

<!-- more -->

> 给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。
>
>   
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/08/18/inorder_1.jpg)
>
> ```
> 输入：root = [1,null,2,3]
> 输出：[1,3,2]
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
>  
>
> **提示：**
>
> - 树中节点数目在范围 `[0, 100]` 内
> - `-100 <= Node.val <= 100`
> 
>  
>
> **进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

### 递归

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        inorderTraversal(root, res);
        return res;
    }

    public void inorderTraversal(TreeNode root, List<Integer> res) {
        if (root != null) {
            inorderTraversal(root.left, res);
            res.add(root.val);
            inorderTraversal(root.right, res);
        }
    }
}
```

### 迭代

先将左子树压入栈中，然后指针指向左孩子重复操作，直到指针为空，说明已无左孩子，从栈中弹出值作为当前指针位置，保存当前值后再将指针指向右孩子，然后重复之前操作。

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }
}
```

