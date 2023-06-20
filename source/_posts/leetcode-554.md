---
title: leetcode-554 砖墙
date: 2023-06-20 10:54:14
tags:
  - 哈希表
categories:
  - leetcode
  - 算法
top:
---

> [554. 砖墙](https://leetcode.cn/problems/brick-wall/description/)
>
> 
>
> 你的面前有一堵矩形的、由 `n` 行砖块组成的砖墙。这些砖块高度相同（也就是一个单位高）但是宽度不同。每一行砖块的宽度之和相等。
>
>    你现在要画一条 **自顶向下** 的、穿过 **最少** 砖块的垂线。如果你画的线只是从砖块的边缘经过，就不算穿过这块砖。**你不能沿着墙的两个垂直边缘之一画线，这样显然是没有穿过一块砖的。**
>
> 给你一个二维数组 `wall` ，该数组包含这堵墙的相关信息。其中，`wall[i]` 是一个代表从左至右每块砖的宽度的数组。你需要找出怎样画才能使这条线 **穿过的砖块数量最少** ，并且返回 **穿过的砖块数量** 。
>
>  
> 
> **示例 1：**
> 
> **![image.png](https://images.orkva.com/images/2023/06/20/1619762681-rvgTEO-image.png)**
> 
> ```
> 输入：wall = [[1,2,2,1],[3,1,2],[1,3,2],[2,4],[3,1,2],[1,3,1,1]]
> 输出：2
> ```
> 
> **示例 2：**
> 
> ```
> 输入：wall = [[1],[1],[1]]
> 输出：3
> ```
> 
>  
> 
> **提示：**
> 
>    - `n == wall.length`
> - `1 <= n <= 104`
> - `1 <= wall[i].length <= 104`
> - `1 <= sum(wall[i].length) <= 2 * 104`
> - 对于每一行 `i` ，`sum(wall[i])` 是相同的
> - `1 <= wall[i][j] <= 231 - 1`

这道题目相对来说比较简单，思路也比较清晰，只需要计算出所有位置上墙的个数，然后取最小值即可。

```java
public int leastBricks(List<List<Integer>> wall) {
    int allWallWidth = 0;
    for (Integer wallWithe : wall.get(0)) {
        allWallWidth += wallWithe;
    }

    if (allWallWidth <= 1) {
        return wall.size();
    }

    int[] overlap = new int[allWallWidth];
    for (List<Integer> wallLine : wall) {
        int startGap = 0;
        for (Integer wallWithe : wallLine) {
            for (int i = startGap + 1; i < startGap + wallWithe; i++) {
                overlap[i] += 1;
            }
            startGap += wallWithe;
        }
    }

    int through = wall.size();
    for (int i = 1; i < overlap.length; i++) {
        through = Math.min(through, overlap[i]);
    }
    return through;
}
```

那么问题来了，有这样的一个测试用例。

> [[100000000],[100000000],[100000000]]

很显然，如果是计算墙壁的厚度，在这个测试用例上不可避免的会导致内存溢出，我们还是需要想其他的思路。

既然计算墙壁的厚度会导致内存溢出，那么我们可不可以计算空隙的数量，然后用总厚度减去最大的间隙？

显然是可以的。

```java
public int leastBricks(List<List<Integer>> wall) {
    Map<Integer, Integer> gapMap = new HashMap<>();
    for (List<Integer> wallLine : wall) {
        int rightEdge = 0;
        for (Integer wallWidth : wallLine) {
            rightEdge += wallWidth;
            Integer gapCount = gapMap.getOrDefault(rightEdge, 0);
            gapMap.put(rightEdge, gapCount + 1);
        }
        // 不计算两侧的空隙
        gapMap.remove(rightEdge);
    }

    int maxGapCount = 0;
    for (Integer gapCount : gapMap.values()) {
        maxGapCount = Math.max(gapCount, maxGapCount);
    }
    return wall.size() - maxGapCount;
}
```

