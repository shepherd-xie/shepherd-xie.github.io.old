---
title: 849. 到最近的人的最大距离
date: 2023-08-22 14:46:50
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

这道题对于社恐患者相当友好，十分怀疑灵感来自男厕所。

https://leetcode.cn/problems/maximize-distance-to-closest-person/description/

<!-- more -->

> 给你一个数组 `seats` 表示一排座位，其中 `seats[i] = 1` 代表有人坐在第 `i` 个座位上，`seats[i] = 0` 代表座位 `i` 上是空的（**下标从 0 开始**）。
>
> 至少有一个空座位，且至少有一人已经坐在座位上。
>
> 亚历克斯希望坐在一个能够使他与离他最近的人之间的距离达到最大化的座位上。
>
> 返回他到离他最近的人的最大距离。
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/08/22/distance.jpg)
>
> ```
> 输入：seats = [1,0,0,0,1,0,1]
> 输出：2
> 解释：
> 如果亚历克斯坐在第二个空位（seats[2]）上，他到离他最近的人的距离为 2 。
> 如果亚历克斯坐在其它任何一个空位上，他到离他最近的人的距离为 1 。
> 因此，他到离他最近的人的最大距离是 2 。 
> ```
>
> **示例 2：**
>
> ```
> 输入：seats = [1,0,0,0]
> 输出：3
> 解释：
> 如果亚历克斯坐在最后一个座位上，他离最近的人有 3 个座位远。
> 这是可能的最大距离，所以答案是 3 。
> ```
>
> **示例 3：**
>
> ```
> 输入：seats = [0,1]
> 输出：1
> ```
>
>  
>
> **提示：**
>
> - `2 <= seats.length <= 2 * 104`
> - `seats[i]` 为 `0` 或 `1`
> - 至少有一个 **空座位**
> - 至少有一个 **座位上有人**

简单的说便是求一个数组中连续最多个数的 0 ，但是对于两端需要特殊处理。

我们定义一个 `closest` 表示距离当前位置最近的上一个座位。

1. 首先我们需要初始化 `closest` ，找到第一个有人坐的位置，然后初始化 `res = closest` ，即左侧边界距离第一个有人位置的距离。
2. 从 `closest + 1` 开始遍历数组，当 `seats[i] == 1` 时，比较当前位置到 `closest` 距离的一半与 `res` 的大小，取较大的一个作为 `res` 的新值。
3. 循环操作，直到数组遍历完。
4. 然后我们再计算 `seats.length - 1 - cloeset` 与 `res` 的大小，并取较大值作为 `res` 的新值，即右侧边界距离最后一个有人位置的距离。

```java
class Solution {
    public int maxDistToClosest(int[] seats) {
        int distence = 0;
        int closest = 0;
        while (seats[closest] == 0) {
            closest ++;
        }
        distence = closest;
        for (int i = closest + 1; i < seats.length; i ++) {
            if (seats[i] == 1) {
                distence = Math.max((i - closest) / 2, distence);
                closest = i;
            }
        }
        return Math.max(seats.length - 1 - closest, distence);
    }
}
```



