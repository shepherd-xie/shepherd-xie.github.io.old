---
title: 963. 最小面积矩形 II
date: 2023-07-19 10:47:47
tags:
  - 数学
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/minimum-area-rectangle-ii/description/

<!-- more -->

> 给定在 xy 平面上的一组点，确定由这些点组成的任何矩形的最小面积，其中矩形的边**不一定平行于** x 轴和 y 轴。
>
> 如果没有任何矩形，就返回 0。
>
>  
>
> **示例 1：**
>
> **![img](https://images.orkva.com/images/2023/07/19/1a.png)**
>
> ```
> 输入：[[1,2],[2,1],[1,0],[0,1]]
> 输出：2.00000
> 解释：最小面积的矩形出现在 [1,2],[2,1],[1,0],[0,1] 处，面积为 2。
> ```
>
> **示例 2：**
>
> ![img](https://images.orkva.com/images/2023/07/19/2.png)
>
> ```
> 输入：[[0,1],[2,1],[1,1],[1,0],[2,0]]
> 输出：1.00000
> 解释：最小面积的矩形出现在 [1,0],[1,1],[2,1],[2,0] 处，面积为 1。
> ```
>
> **示例 3：**
>
> ![img](https://images.orkva.com/images/2023/07/19/3.png)
>
> ```
> 输入：[[0,3],[1,2],[3,1],[1,3],[2,1]]
> 输出：0
> 解释：没法从这些点中组成任何矩形。
> ```
>
> **示例 4：**
>
> **![img](https://images.orkva.com/images/2023/07/19/4c.png)**
>
> ```
> 输入：[[3,1],[1,1],[0,1],[2,1],[3,3],[3,2],[0,2],[2,3]]
> 输出：2.00000
> 解释：最小面积的矩形出现在 [2,1],[2,3],[3,3],[3,1] 处，面积为 2。
> ```
>
>  
>
> **提示：**
>
> 1. `1 <= points.length <= 50`
> 2. `0 <= points[i][0] <= 40000`
> 3. `0 <= points[i][1] <= 40000`
> 4. 所有的点都是不同的。
> 5. 与真实值误差不超过 `10^-5` 的答案将视为正确结果。

没有什么思路的就直接暴力穷举好了，比较麻烦的是如何判断矩形，这里使用的是 [[find if 4 points on a plane form a rectangle?](https://stackoverflow.com/questions/2303278/find-if-4-points-on-a-plane-form-a-rectangle)](https://stackoverflow.com/questions/2303278/find-if-4-points-on-a-plane-form-a-rectangle) 的解答思路。

```java
class Solution {
    public double minAreaFreeRect(int[][] points) {
        double minArea = 0;
        for (int i = 0; i < points.length; i++) {
            for (int j = i + 1; j < points.length; j++) {
                for (int k = j + 1; k < points.length; k++) {
                    for (int l = k + 1; l < points.length; l++) {
                        if (isRectangle(points[i][0], points[i][1], points[j][0], points[j][1], points[k][0], points[k][1], points[l][0], points[l][1])) {
                            if (minArea == 0) {
                                minArea = getS(points[i][0], points[i][1], points[j][0], points[j][1], points[k][0], points[k][1], points[l][0], points[l][1]);
                            } else {
                                minArea = Math.min(minArea, getS(points[i][0], points[i][1], points[j][0], points[j][1], points[k][0], points[k][1], points[l][0], points[l][1]));
                            }
                        }
                    }
                }
            }
        }
        return minArea;
    }

    public boolean isRectangle(int x1, int y1,
                               int x2, int y2,
                               int x3, int y3,
                               int x4, int y4) {
        double cx = (x1 + x2 + x3 + x4) / 4.0;
        double cy = (y1 + y2 + y3 + y4) / 4.0;
        double dd1 = (cx - x1) * (cx - x1) + (cy - y1) * (cy - y1);
        double dd2 = (cx - x2) * (cx - x2) + (cy - y2) * (cy - y2);
        double dd3 = (cx - x3) * (cx - x3) + (cy - y3) * (cy - y3);
        double dd4 = (cx - x4) * (cx - x4) + (cy - y4) * (cy - y4);
        return Math.abs(dd1 - dd2) < 1E-6 && Math.abs(dd1 - dd3) < 1E-6 && Math.abs(dd1 - dd4) < 1E-6;
    }

    public double getS(int x1, int y1,
                       int x2, int y2,
                       int x3, int y3,
                       int x4, int y4) {
        return Math.abs(x1 * y2 + x2 * y3 + x3 * y1 - x1 * y3 - x2 * y1 - x3 * y2) / 2.0 +
                Math.abs(x1 * y3 + x3 * y4 + x4 * y1 - x1 * y4 - x3 * y1 - x4 * y3) / 2.0;
    }

}
```

