---
title: 1572. 矩阵对角线元素的和
date: 2023-08-11 14:53:00
tags:
  - 矩阵
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/matrix-diagonal-sum/description/

<!-- more -->

> - 给你一个正方形矩阵 `mat`，请你返回矩阵对角线元素的和。
>
>   请你返回在矩阵主对角线上的元素和副对角线上且不在主对角线上元素的和。
>
>    
>
>   **示例 1：**
>
>   ![img](https://images.orkva.com/images/2023/08/11/sample_1911.png)
>
>   ```
>   输入：mat = [[1,2,3],
>               [4,5,6],
>               [7,8,9]]
>   输出：25
>   解释：对角线的和为：1 + 5 + 9 + 3 + 7 = 25
>   请注意，元素 mat[1][1] = 5 只会被计算一次。
>   ```
>
>   **示例 2：**
>
>   ```
>   输入：mat = [[1,1,1,1],
>               [1,1,1,1],
>               [1,1,1,1],
>               [1,1,1,1]]
>   输出：8
>   ```
>
>   **示例 3：**
>
>   ```
>   输入：mat = [[5]]
>   输出：5
>   ```
>
>    
>
>   **提示：**
>
>   - `n == mat.length == mat[i].length`
>   - `1 <= n <= 100`
>   - `1 <= mat[i][j] <= 100`

由于矩形是正方形，所以不用考虑对角线经过的点，直接模拟计算即可。

1. 设 left=0 right=n-1 为两条对角线与当前行的交点，n 为矩阵长度
2. 按行遍历矩阵，每经过一行 left++ right--
3. 结束后对 n 为奇数的矩阵做特殊处理，减去中心值

```java
class Solution {
    public int diagonalSum(int[][] mat) {
        int n = mat.length;
        int left = 0;
        int right = n - 1;
        int res = 0;
        for (int[] nums : mat) {
            res += nums[left ++] + nums[right --];
        }
        return res - mat[n / 2][n / 2] * (n & 1);
    }
}
```



