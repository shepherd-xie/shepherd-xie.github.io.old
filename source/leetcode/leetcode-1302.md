---
title: 1302. 层数最深叶子节点的和
date: 2023-07-14 14:41:17
tags:
  - 二叉树
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/deepest-leaves-sum/description/

<!-- more -->

> 给你一棵二叉树的根节点 `root` ，请你返回 **层数最深的叶子节点的和** 。
>
>  
> 
> **示例 1：**
> 
>  **![img](https://images.orkva.com/images/2023/07/14/1483_ex1.png)**
> 
>```
> 输入：root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
>输出：15
>  ```
>
>  **示例 2：**
>
> ```
>输入：root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
> 输出：19
>```
> 
>  
> 
> **提示：**
> 
> - 树中节点数目在范围 `[1, 104]` 之间。
> - `1 <= Node.val <= 100`

通过率 85.5% 的 medium ，行吧，姑且算它是 medium 。

审一遍题，第一反应利用广度优先搜索更合适一些。

## 解法一：广度优先搜索

```java
class Solution {
    public int deepestLeavesSum(TreeNode root) {
        int res = 0;
        List<TreeNode> list = new ArrayList<>();
        list.add(root);
        while (!list.isEmpty()) {
            int sum = 0;
            List<TreeNode> nextList = new ArrayList<>();
            for (TreeNode treeNode : list) {
                sum += treeNode.val;
                if (treeNode.left != null) {
                    nextList.add(treeNode.left);
                }
                if (treeNode.right != null) {
                    nextList.add(treeNode.right);
                }
            }
            res = sum;
            list = nextList;
        }
        return res;
    }
}
```

按照树的层级进行遍历，当没有下一个层级时返回最后的数据即可。

官方解法中将 List 改为了 Queue ，可以重复进行使用，避免了多次申请内存的问题。

```java
int size = queue.size();
for (int i = 0; i < size; i++)
```

## 解法二：深度优先搜索

其实最开始我并不觉得深度优先搜索适合这道题，当然在看了讨论之后还是会有一些新的看法。

深度优先的思想是利用全局变量记录当前遍历的最深层，如果大于最深层则更新层深，如果等于最深层则添加数据。

```java
class Solution {
    int maxLevel = -1;
    int sum = 0;

    public int deepestLeavesSum(TreeNode root) {
        dfs(root, 0);
        return sum;
    }

    public void dfs(TreeNode node, int level) {
        if (node == null) {
            return;
        }
        if (level > maxLevel) {
            maxLevel = level;
            sum = node.val;
        } else if (level == maxLevel) {
            sum += node.val;
        }
        dfs(node.left, level + 1);
        dfs(node.right, level + 1);
    }
}
```

