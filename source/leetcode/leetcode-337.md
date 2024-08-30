---
title: 337. 打家劫舍 III
date: 2023-09-18 15:11:32
tags:
  - 二叉树
  - 动态规划
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/house-robber-iii/description/

<!-- more -->

> 小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。
>
> 除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。
>
> 给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。
>
>  
>
> **示例 1:**
>
> ![img](https://images.orkva.com/images/2023/09/18/rob1-tree.jpg)
>
> ```
> 输入: root = [3,2,3,null,3,null,1]
> 输出: 7 
> 解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
> ```
>
> **示例 2:**
>
> ![img](https://images.orkva.com/images/2023/09/18/rob2-tree.jpg)
>
> ```
> 输入: root = [3,4,5,1,3,null,1]
> 输出: 9
> 解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
> ```
>
>  
>
> **提示：**
>
> 
>
> - 树的节点数在 `[1, 104]` 范围内
> - `0 <= Node.val <= 104`

显然这个年代的盗贼技术含量也太高了，还能够意识到 _这个地方的所有房屋的排列类似于一棵二叉树_ ，什么时候都要学习啊。

显然这道题是一个动态规划的问题，父节点的盗窃与否直接决定子节点是否可以盗窃，我们记是否盗窃该节点为 `rob(node)` 与 `norob(node)` ，对节点 `node` 则有

* 如果盗窃该节点，那么就无法盗窃该节点的子节点，最大盗窃金额为 `node.val + norob(node.left) + norob(node.right)`
* 如果不盗窃该节点，其子节点可以盗窃也可以盗窃，最大盗窃金额为 `max(rob(node.left), norob(node.left)) + max(rob(node.right), norob(node.right))`

```java
class Solution {
    public int rob(TreeNode root) {
        int[] res = dfs(root);
        return Math.max(res[0], res[1]);
    }

    public int[] dfs(TreeNode node) {
        if (node == null) {
            return new int[]{0, 0};
        }

        int[] left = dfs(node.left);
        int[] right = dfs(node.right);
        int rob = node.val + left[1] + right[1];
        int noRob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        return new int[]{rob, noRob};
    }
}
