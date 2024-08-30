---
title: 35. 搜索插入位置
date: 2023-12-06 13:20:39
tags:
  - 数组
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/search-insert-position/

<!-- more -->

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
> 请必须使用时间复杂度为 `O(log n)` 的算法。
>
>  
>
> **示例 1:**
>
> ```
> 输入: nums = [1,3,5,6], target = 5
> 输出: 2
> ```
>
> **示例 2:**
>
> ```
> 输入: nums = [1,3,5,6], target = 2
> 输出: 1
> ```
>
> **示例 3:**
>
> ```
> 输入: nums = [1,3,5,6], target = 7
> 输出: 4
> ```
>
>  
>
> **提示:**
>
> - `1 <= nums.length <= 104`
> - `-104 <= nums[i] <= 104`
> - `nums` 为 **无重复元素** 的 **升序** 排列数组
> - `-104 <= target <= 104`

既然是有序数组，又要求 `log n` 时间复杂度，那一定是使用二分法。不过还是需要注意边界问题，比如 `while` 中用 `<` 还是 `<=` ，以及缩小边界时使用 `left = mid` 还是 `left = mid + 1` 。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
}
```
