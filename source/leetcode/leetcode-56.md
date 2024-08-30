---
title: 56. 合并区间
date: 2023-08-29 15:43:48
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/merge-intervals/description/

<!-- more -->

> 以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。
>
>  
>
>  **示例 1：**
>
> ```
>输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
> 输出：[[1,6],[8,10],[15,18]]
>解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
> ```
> 
> **示例 2：**
> 
> ```
> 输入：intervals = [[1,4],[4,5]]
> 输出：[[1,5]]
> 解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
> ```
>
>  
>
> **提示：**
>
> - `1 <= intervals.length <= 104`
> - `intervals[i].length == 2`
> - `0 <= starti <= endi <= 104`

首先我们来思考两个区间可能出现的所有情况。

![image.png](https://images.orkva.com/images/2023/08/29/91d75169b1cdb15560d361f8cb7050adfe7906c955afbe8846b92d1beba8a0d7-image.png)

情况共有三种，相交、包含和无关。其中需要处理合并的为相交与包含关系。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        // 首先我们对数组进行排序，保证区间起点的递增
        intervals = Arrays.stream(intervals).sorted(Comparator.comparingInt(o -> o[0])).toArray(int[][]::new);
        // 定义两个边界，表示我们将要处理的区间边界，初始值为第一个边界
        int left = intervals[0][0];
        int right = intervals[0][1];
        List<int[]> res = new ArrayList<>();

        for (int i = 1; i < intervals.length; i ++) {
            // 表示两个区间无重合
            if (intervals[i][0] > right) {
                // 将之前的区间添加至返回值
                res.add(new int[]{left, right});
                // 将当前区间作为新的待处理区间
                left = intervals[i][0];
                right = intervals[i][1];
                // 表示两个区间相交
            } else if (intervals[i][0] >= left && intervals[i][1] > right) {
                // 合并区间
                right = intervals[i][1];
            }
            // 最后是区间包含，由于已经进行排序，必然是待处理区间包含当前区间，所以不做处理
        }
        // 最后一个未处理的区间加入返回值
        res.add(new int[]{left, right});
        return res.toArray(new int[0][0]);
    }
}
```

