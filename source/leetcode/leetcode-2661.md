---
title: 2661. 找出叠涂元素
date: 2023-12-01 10:13:34
tags:
  - 矩阵
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/first-completely-painted-row-or-column/description/

<!-- more -->

>给你一个下标从 **0** 开始的整数数组 `arr` 和一个 `m x n` 的整数 **矩阵** `mat` 。`arr` 和 `mat` 都包含范围 `[1，m * n]` 内的 **所有** 整数。
>
>从下标 `0` 开始遍历 `arr` 中的每个下标 `i` ，并将包含整数 `arr[i]` 的 `mat` 单元格涂色。
>
>请你找出 `arr` 中在 `mat` 的某一行或某一列上都被涂色且下标最小的元素，并返回其下标 `i` 。
>
> 
>
>**示例 1：**
>
>![image explanation for example 1](https://assets.leetcode.com/uploads/2023/01/18/grid1.jpg)
>
>```
>输入：arr = [1,3,4,2], mat = [[1,4],[2,3]]
>输出：2
>解释：遍历如上图所示，arr[2] 在矩阵中的第一行或第二列上都被涂色。
>```
>
>**示例 2：**
>
>![image explanation for example 2](https://assets.leetcode.com/uploads/2023/01/18/grid2.jpg)
>
>```
>输入：arr = [2,8,7,4,1,3,5,6,9], mat = [[3,2,5],[1,4,6],[8,7,9]]
>输出：3
>解释：遍历如上图所示，arr[3] 在矩阵中的第二列上都被涂色。
>```
>
> 
>
>**提示：**
>
>- `m == mat.length`
>- `n = mat[i].length`
>- `arr.length == m * n`
>- `1 <= m, n <= 105`
>- `1 <= m * n <= 105`
>- `1 <= arr[i], mat[r][c] <= m * n`
>- `arr` 中的所有整数 **互不相同**
>- `mat` 中的所有整数 **互不相同**

要求出最先涂满一列或者一行的对应数组下标。我们可以反过来思考，当数组 `arr` 遍历到什么时候，某行或某列才会被填满？答案是当这一行/列中的元素在 `arr` 中对应的最大的下标。

所以，我们可以先找出每一行每一列会在什么时候填满，然后从中取最小值即为记过。

```java
class Solution {
    public int firstCompleteIndex(int[] arr, int[][] mat) {
        int[] memo = new int[arr.length + 1];
        for (int i = 0; i < arr.length; i++) {
            memo[arr[i]] = i;
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < mat.length; i++) {
            int maxIdx = 0;
            for (int num : mat[i]) {
                maxIdx = Math.max(maxIdx, memo[num]);
            }
            ans = Math.min(ans, maxIdx);
        }
        for (int i = 0; i < mat[0].length; i++) {
            int maxIdx = 0;
            for (int j = 0; j < mat.length; j++) {
                maxIdx = Math.max(maxIdx, memo[mat[j][i]]);
            }
            ans = Math.min(ans, maxIdx);
        }
        return ans;
    }
}
```

