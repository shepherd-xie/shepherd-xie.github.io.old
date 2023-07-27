---
title: 54. 螺旋矩阵
date: 2023-06-29 15:02:31
tags:
  - 数组
  - 矩阵
  - medium
  - leetcode
categories:
  - 算法
top:
---

[54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/description/)

<!-- more -->

>
> 给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)
>
> ```
> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
> 输出：[1,2,3,6,9,8,7,4,5]
> ```
>
> **示例 2：**
>
> ![img](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)
>
> ```
> 输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
> 输出：[1,2,3,4,8,12,11,10,9,5,6,7]
> ```
>
>  
>
> **提示：**
>
> - `m == matrix.length`
> - `n == matrix[i].length`
> - `1 <= m, n <= 10`
> - `-100 <= matrix[i][j] <= 100`

貌似是在上大学时也接触过这一类型的题目，看似简单，只需要按顺序输出即可，关键点在于二维数组的边界条件，即临界点的判断。

* 第一步，我们先分别定义四个变量 `top bottom left right` 来代表当前矩阵的四个边界，以后我们所有的计算就只会在边界之中进行。
* 第二步，按照题目的顺序，以顺时针方式遍历矩阵。
* 第三步，每遍历完一条边，便将这条边所对应的边界进行压缩，得到一个边界更小的矩阵。
* 第四步，重复第二步和第三步，直到相对的边界出现冲突 `top < bottom` `left > right` 表示循环结束。

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();
        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;

        while (true) {
            if (top > bottom) break;
            for (int i = left; i <= right; i++)
                result.add(matrix[top][i]);
            top ++;

            if (right < left) break;
            for (int i = top; i <= bottom; i++)
                result.add(matrix[i][right]);
            right --;

            if (bottom < top) break;
            for (int i = right; i >= left; i--)
                result.add(matrix[bottom][i]);
            bottom --;

            if (left > right) break;
            for (int i = bottom; i >= top; i--)
                result.add(matrix[i][left]);
            left ++;
        }

        return result;
    }
}
```

