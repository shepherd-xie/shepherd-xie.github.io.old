---
title: 1305. 两棵二叉搜索树中的所有元素
date: 2023-08-04 11:28:53
tags:
  - 二叉树
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/all-elements-in-two-binary-search-trees/

<!-- more -->

> 给你 `root1` 和 `root2` 这两棵二叉搜索树。请你返回一个列表，其中包含 **两棵树** 中的所有整数并按 **升序** 排序。.
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/08/04/q2-e1.png)
> 
>```
> 输入：root1 = [2,1,4], root2 = [1,0,3]
>输出：[0,1,1,2,3,4]
> ```
>
> **示例 2：**
> 
> ![img](https://images.orkva.com/images/2023/08/04/q2-e5.png)
> 
> ```
> 输入：root1 = [1,null,8], root2 = [8,1]
> 输出：[1,1,8,8]
> ```
>
>  
>
> **提示：**
> 
> - 每棵树的节点数在 `[0, 5000]` 范围内
> - `-105 <= Node.val <= 105`

既然是二叉搜索树，那么我们按照中序遍历即可得到两个已排序的数组，再将两个数组合并即可。

```java
class Solution {
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        List<Integer> list1 = new ArrayList();
        List<Integer> list2 = new ArrayList();
        dfs(root1, list1);
        dfs(root2, list2);

        List<Integer> res = new ArrayList();
        for (int i = 0, j = 0; i < list1.size() || j < list2.size(); ) {
            int num1 = i < list1.size() ? list1.get(i) : Integer.MAX_VALUE;
            int num2 = j < list2.size() ? list2.get(j) : Integer.MAX_VALUE;
            if (num1 < num2) {
                res.add(num1);
                i ++;
            } else {
                res.add(num2);
                j ++;
            }
        }
        return res;
    }

    public void dfs(TreeNode node, List<Integer> list) {
        if (node == null) return;
        dfs(node.left, list);
        list.add(node.val);
        dfs(node.right, list);
    }
}
```

