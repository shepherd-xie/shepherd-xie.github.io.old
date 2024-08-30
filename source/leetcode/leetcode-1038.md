---
title: 1038. 从二叉搜索树到更大和树
date: 2023-12-04 10:14:22
tags:
  - 树
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/binary-search-tree-to-greater-sum-tree/description/

<!-- more -->

> 给定一个二叉搜索树 `root` (BST)，请将它的每个节点的值替换成树中大于或者等于该节点值的所有节点值之和。
>
> 提醒一下， *二叉搜索树* 满足下列约束条件：
>
> - 节点的左子树仅包含键 **小于** 节点键的节点。
>- 节点的右子树仅包含键 **大于** 节点键的节点。
> - 左右子树也必须是二叉搜索树。
>
>  
>  
> **示例 1：**
> 
> **![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)**
> 
> ```
> 输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
> 输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
> ```
> 
> **示例 2：**
> 
>```
> 输入：root = [0,null,1]
>输出：[1,null,1]
> ```
> 
>  
>  
> **提示：**
> 
> - 树中的节点数在 `[1, 100]` 范围内。
> - `0 <= Node.val <= 100`
> - 树中的所有值均 **不重复** 。

由于是二叉搜索树，所以可以使用深度优先遍历的方式。

1. 遍历右子树
2. 更新 val 值
3. 更新节点值
4. 遍历左子树
5. 递归更新

```java
class Solution {
    int val = 0;

    public TreeNode bstToGst(TreeNode root) {
        if (root == null) {
            return null;
        }
        bstToGst(root.right);
        val += root.val;
        root.val = val;
        bstToGst(root.left);
        return root;
    }
}
```

