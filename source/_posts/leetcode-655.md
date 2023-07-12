---
title: 655. 输出二叉树
date: 2023-07-12 13:25:33
tags:
  - 树
  - 二叉树
  - 深度优先搜索
  - 广度优先搜索
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/print-binary-tree/

<!-- more -->

> 给你一棵二叉树的根节点 `root` ，请你构造一个下标从 **0** 开始、大小为 `m x n` 的字符串矩阵 `res` ，用以表示树的 **格式化布局** 。构造此格式化布局矩阵需要遵循以下规则：
>
> - 树的 **高度** 为 `height` ，矩阵的行数 `m` 应该等于 `height + 1` 。
> - 矩阵的列数 `n` 应该等于 $2 ^ {height+1} - 1$ 。
> - **根节点** 需要放置在 **顶行** 的 **正中间** ，对应位置为 `res[0][(n-1)/2]` 。
> - 对于放置在矩阵中的每个节点，设对应位置为 `res[r][c]` ，将其左子节点放置在 $res[r+1][c-2^{height-r-1}]$ ，右子节点放置在 $res[r+1][c+2^{height-r-1}]$ 。
> - 继续这一过程，直到树中的所有节点都妥善放置。
> - 任意空单元格都应该包含空字符串 `""` 。
>
> 返回构造得到的矩阵 `res` 。
>
>  
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/07/12/print1-tree.jpg)
>
> ```
> 输入：root = [1,2]
> 输出：
> [["","1",""],
>  ["2","",""]]
> ```
>
> **示例 2：**
>
> ![img](https://images.orkva.com/images/2023/07/12/print2-tree.jpg)
>
> ```
> 输入：root = [1,2,3,null,4]
> 输出：
> [["","","","1","","",""],
>  ["","2","","","","3",""],
>  ["","","4","","","",""]]
> ```
>
>  
>
> **提示：**
>
> - 树中节点数在范围 `[1, 210]` 内
> - `-99 <= Node.val <= 99`
> - 树的深度在范围 `[1, 10]` 内

有段日子没见过这么标准的 medium 了。

首先分析题目，要求我们将一个二叉树格式化输出到一个矩阵中，而且十分贴心的告诉了我们每个节点应该放在的位置，那么问题就比较简单了。

* 首先，遍历二叉树得到树的深度（高度），在这里默认根节点的深度为 0 。
* 得到深度以后，再次遍历二叉树，并且按照题目要求填充矩阵。

好像也挺简单的。

```java
class Solution {
    public List<List<String>> printTree(TreeNode root) {
        List<List<String>> res = new ArrayList<>();
        int depth = getDepth(root, 0); // 获取树深度
        int width = (1 << depth + 1) - 1;
        for (int i = 0; i < depth + 1; i++) {
            List<String> list = new ArrayList<>();
            for (int j = 0; j < width; j++) {
                list.add(""); // 将矩阵初始化为 ""
            }
            res.add(list);
        }
        // 按照题目规则填充矩阵
        filling(res, root, 0, (width - 1) / 2, depth);
        return res;
    }

    public void filling(List<List<String>> res, TreeNode node, int depth, int column, int treeHeight) {
        List<String> row = res.get(depth);
        row.set(column, String.valueOf(node.val));

        if (node.left != null) {
            filling(res, node.left, depth + 1, column - (1 << (treeHeight - depth - 1)), treeHeight);
        }
        if (node.right != null) {
            filling(res, node.right, depth + 1, column + (1 << (treeHeight - depth - 1)), treeHeight);
        }
    }

    public int getDepth(TreeNode node, int depth) {
        if (node.left == null && node.right == null) {
            return depth;
        }
        int depthLeft = depth;
        int depthRight = depth;
        if (node.left != null) {
            depthLeft = getDepth(node.left, depth + 1);
        }
        if (node.right != null) {
            depthRight = getDepth(node.right, depth + 1);
        }
        return Math.max(depthLeft, depthRight);
    }
}
```

