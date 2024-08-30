---
title: 498. 对角线遍历
date: 2023-12-12 10:33:48
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/diagonal-traverse/description/

<!-- more -->

> 给你一个大小为 `m x n` 的矩阵 `mat` ，请以对角线遍历的顺序，用一个数组返回这个矩阵中的所有元素。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)
> 
> ```
> 输入：mat = [[1,2,3],[4,5,6],[7,8,9]]
>输出：[1,2,4,7,5,3,6,8,9]
> ```
>
> **示例 2：**
>
> ```
>输入：mat = [[1,2],[3,4]]
> 输出：[1,2,3,4]
> ```
> 
>  
> 
> **提示：**
> 
> - `m == mat.length`
> - `n == mat[i].length`
> - `1 <= m, n <= 104`
> - `1 <= m * n <= 104`
> - `-105 <= mat[i][j] <= 105`

类似此类二维数组，最重要的就是要寻找到他们之间的规律。

当我们将所有坐标标出时可以发现，每一次斜向遍历时横纵坐标之和保持不变，且下一次遍历横纵之和必然比此次多 1 。我们可以按遍历方向将过程分为两类：

* 当和为偶数时，由左下到右上遍历，过程中横坐标增加纵坐标减少
* 当和为奇数时，由右上到左下遍历，过程中纵坐标增加横坐标减少

```java
class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int[] ans = new int[m * n];
        int pos = 0;

        for (int sum = 0; sum < m + n; sum++) {
            if (sum % 2 == 0) {
                int x = sum < m ? sum : m - 1;
                int y = sum - x;
                while (x >= 0 && y < n) {
                    ans[pos++] = mat[x--][y++];
                }
            } else {
                int y = sum < n ? sum : n - 1;
                int x = sum - y;
                while (y >= 0 && x < m) {
                    ans[pos++] = mat[x++][y--];
                }
            }
        }

        return ans;
    }
}
```

