---
title: 1749. 任意子数组和的绝对值的最大值
date: 2023-08-08 10:46:41
tags:
  - 动态规划
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/maximum-absolute-sum-of-any-subarray/

<!-- more -->

> 给你一个整数数组 `nums` 。一个子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` 的 **和的绝对值** 为 `abs(numsl + numsl+1 + ... + numsr-1 + numsr)` 。
>
> 请你找出 `nums` 中 **和的绝对值** 最大的任意子数组（**可能为空**），并返回该 **最大值** 。
>
> `abs(x)` 定义如下：
>
> - 如果 `x` 是负整数，那么 `abs(x) = -x` 。
> - 如果 `x` 是非负整数，那么 `abs(x) = x` 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [1,-3,2,3,-4]
> 输出：5
> 解释：子数组 [2,3] 和的绝对值最大，为 abs(2+3) = abs(5) = 5 。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [2,-5,1,-4,3,-2]
> 输出：8
> 解释：子数组 [-5,1,-4] 和的绝对值最大，为 abs(-5+1-4) = abs(-8) = 8 。
> ```
>
>  
>
> **提示：**
>
> - `1 <= nums.length <= 105`
> - `-104 <= nums[i] <= 104`

像这种连续数组和的问题，下意识的就可以向前缀和方向去靠。

由于子数组的和就是前缀和的差，那么和的最大值即为差的最大值，我们只要找到前缀和的最大值和最小值即可。

* 如果最大值出现在最小值的右侧，那么和为最大子数组和
* 如果最大值出现在最小值的左侧，那么和为最小子数组和的绝对值

```java
class Solution {
    public int maxAbsoluteSum(int[] nums) {
        int n = nums.length;
        int[] preSum = new int[n + 1];
        int minPreSum = 0;
        int maxPreSum = 0;
        for (int i = 0; i < n; i ++) {
            preSum[i + 1] = preSum[i] + nums[i];
            minPreSum = Math.min(preSum[i + 1], minPreSum);
            maxPreSum = Math.max(preSum[i + 1], maxPreSum);
        }
        return maxPreSum - minPreSum;
    }
}
```

可以看出根本不必维护前缀和数组，我们需要的只是所有的前缀和。

```java
class Solution {
    public int maxAbsoluteSum(int[] nums) {
        int n = nums.length;
        int preSum = 0;
        int minPreSum = 0;
        int maxPreSum = 0;
        for (int i = 0; i < n; i ++) {
            preSum += nums[i];
            minPreSum = Math.min(preSum, minPreSum);
            maxPreSum = Math.max(preSum, maxPreSum);
        }
        return maxPreSum - minPreSum;
    }
}
```

