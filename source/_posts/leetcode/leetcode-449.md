---
title: 449. 序列化和反序列化二叉搜索树
date: 2023-09-04 09:32:21
tags:
  - 树
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/serialize-and-deserialize-bst/description/

<!-- more -->

> 序列化是将数据结构或对象转换为一系列位的过程，以便它可以存储在文件或内存缓冲区中，或通过网络连接链路传输，以便稍后在同一个或另一个计算机环境中重建。
>
> 设计一个算法来序列化和反序列化 **二叉搜索树** 。 对序列化/反序列化算法的工作方式没有限制。 您只需确保二叉搜索树可以序列化为字符串，并且可以将该字符串反序列化为最初的二叉搜索树。
>
> **编码的字符串应尽可能紧凑。**
>
>  
>
> **示例 1：**
>
> ```
> 输入：root = [2,1,3]
> 输出：[2,1,3]
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
> - 树中节点数范围是 `[0, 104]`
> - `0 <= Node.val <= 104`
> - 题目数据 **保证** 输入的树是一棵二叉搜索树。

如何压缩一棵树，那么最先想到的解法一定是先序遍历，由于是一颗 BTree ，所以我们只要将输出的序列重新构建为一棵树即可。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) {
            return "";
        }
        StringBuilder sb = new StringBuilder();
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            sb.append(node.val).append(',');
            if (node.left != null) {
                stack.push(node.left);
            }
            if (node.right != null) {
                stack.push(node.right);
            }
        }
        return sb.deleteCharAt(sb.length() - 1).toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.isEmpty()) {
            return null;
        }
        TreeNode root = null;
        for (String s : data.split(",")) {
            TreeNode newNode = new TreeNode(Integer.valueOf(s));
            if (root == null) {
                root = newNode;
                continue;
            }
            TreeNode node = root;
            while (node != null) {
                if (newNode.val < node.val) {
                    if (node.left == null) {
                        node.left = newNode;
                        break;
                    } else {
                        node = node.left;
                    }
                } else {
                    if (node.right == null) {
                        node.right = newNode;
                        break;
                    } else {
                        node = node.right;
                    }
                }
            }
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// String tree = ser.serialize(root);
// TreeNode ans = deser.deserialize(tree);
// return ans;
```

