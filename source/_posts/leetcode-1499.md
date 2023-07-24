---
title: 1499. 满足不等式的最大值
date: 2023-07-21 11:23:11
tags:
  - 未理清
  - 单调队列
  - hard
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/max-value-of-equation/description/

<!-- more -->

> 给你一个数组 `points` 和一个整数 `k` 。数组中每个元素都表示二维平面上的点的坐标，并按照横坐标 x 的值从小到大排序。也就是说 `points[i] = [xi, yi]` ，并且在 `1 <= i < j <= points.length` 的前提下， `xi < xj` 总成立。
>
> 请你找出 `yi + yj + |xi - xj|` 的 **最大值**，其中 `|xi - xj| <= k` 且 `1 <= i < j <= points.length`。
>
> 题目测试数据保证至少存在一对能够满足 `|xi - xj| <= k` 的点。
>
>    
>
>   **示例 1：**
>
>   ```
>   输入：points = [[1,3],[2,0],[5,10],[6,-10]], k = 1
>   输出：4
>   解释：前两个点满足 |xi - xj| <= 1 ，代入方程计算，则得到值 3 + 0 + |1 - 2| = 4 。第三个和第四个点也满足条件，得到值 10 + -10 + |5 - 6| = 1 。
>   没有其他满足条件的点，所以返回 4 和 1 中最大的那个。
>   ```
>
>   **示例 2：**
>
>   ```
>   输入：points = [[0,0],[3,0],[9,2]], k = 3
>   输出：3
>   解释：只有前两个点满足 |xi - xj| <= 3 ，代入方程后得到值 0 + 0 + |0 - 3| = 3 。
>   ```
>
>    
>
>   **提示：**
>
>   - `2 <= points.length <= 10^5`
>   - `points[i].length == 2`
>   - `-10^8 <= points[i][0], points[i][1] <= 10^8`
>   - `0 <= k <= 2 * 10^8`
>   - 对于所有的`1 <= i < j <= points.length` ，`points[i][0] < points[j][0]` 都成立。也就是说，`xi` 是严格递增的。

首先还是根据题目条件写出最简单的暴力解法。

```java
class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        long res = Integer.MIN_VALUE;
        for (int i = 0; i < points.length; i++) {
            for (int j = i + 1; j < points.length; j++) {
                long delta = Math.abs((long) points[i][0] - points[j][0]);
                if (delta > k) {
                    break;
                }
                res = Math.max(res, points[i][1] + points[j][1] + delta);
            }
        }
        return Math.toIntExact(res);
    }
}
```

根据题目描述 `在 1 <= i < j <= points.length 的前提下， xi < xj 总成立` 那么我们可以化简题目 
$$
x_i + y_j + |x_i - x_j| \\
= y_i + y_j + x_j - x_i \\
= (x_j + y_j) + (y_i - x_i)
$$

对于每一组 $(x_j,y_j)$ , 都有唯一的一组 $(y_i - x_i)_{max}$ 可以使上式最大，其中 $i < j$ 且 $x_i \geq x_j - k$ 。

* 单调队列存储二元组 $(x_i,y_i-x_i)$ 。
* 首先把队首的超出范围的数据出队，即 $x_i<x_j-k$ 的数据。
* 然后把 $(x_j,y_j - x_j)$ 入队，入队前如果发现 $y_j - x_j$ 不低于队尾的数据，那么直接弹出队尾。
* 这样维护后，单调队列的 $y_i-x_i$ 从队首到队尾是严格递减的，$y_i - x_i$ 的最大值即为队首的最大值。

```java
class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        int res = Integer.MIN_VALUE;
        Deque<int[]> deque = new ArrayDeque<>();
        for (int[] point : points) {
            int x = point[0];
            int y = point[1];
            while (!deque.isEmpty() && deque.peekFirst()[0] < x - k)
                deque.pollFirst();
            if (!deque.isEmpty())
                res = Math.max(res, x + y + deque.peekFirst()[1]);
            while (!deque.isEmpty() && deque.peekLast()[1] <= y - x)
                deque.pollLast();
            deque.addLast(new int[]{x, y - x});
        }
        return res;
    }
}
```

