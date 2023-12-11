---
title: 48. 旋转图像
date: 2023-12-11 10:50:57
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/rotate-image/description/

<!-- more -->

> 给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。
>
> 你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。
>
>  
>
> **示例 1：**
>
> ![img](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)
>
> ```
> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
> 输出：[[7,4,1],[8,5,2],[9,6,3]]
> ```
>
> **示例 2：**
>
> ![img](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)
>
> ```
> 输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
> 输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
> ```
>
>  
>
> **提示：**
>
> - `n == matrix.length == matrix[i].length`
> - `1 <= n <= 20`
> - `-1000 <= matrix[i][j] <= 1000`

首先需要理清数组旋转 90 度所对应的含义：

![ccw-01-07.001.png](https://pic.leetcode-cn.com/1638557961-AVzCQb-ccw-01-07.001.png)

> 作者：Krahets
> 链接：https://leetcode.cn/problems/rotate-image/
> 来源：力扣（LeetCode）

其次要确定每次转动的边界，每一次遍历的最后一个元素是属于本次遍历还是下一次遍历。

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix[0].length;
        for (int level = 0; level < n / 2; level++) {
            for (int i = level; i < n - 1 - level; i++) {
                int tmp = matrix[level][i];

                matrix[level][i] = matrix[n - 1 - i][level];
                matrix[n - 1 - i][level] = matrix[n - 1 - level][n - 1 - i];
                matrix[n - 1 - level][n - 1 - i] = matrix[i][n - 1 - level];
                matrix[i][n - 1 - level] = tmp;
            }
        }
    }
}
```

