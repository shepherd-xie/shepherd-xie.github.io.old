---
title: 53. 最大子数组和
date: 2023-11-20 10:25:44
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/maximum-subarray/description/

<!-- more -->

> 给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
>
> **子数组** 是数组中的一个连续部分。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出：6
> 解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [1]
> 输出：1
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [5,4,-1,7,8]
> 输出：23
> ```
>
>  
>
> **提示：**
>
> - `1 <= nums.length <= 105`
> - `-104 <= nums[i] <= 104`
>
>  
>
> **进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。

假设 $nums$ 的长度为 $n$ ，下标从 $0$ 到 $n-1$ 。我们用 $presum(i)$ 表示以下标 $i$ 结尾的前 $i$ 个数之和，可以得出 $presum(i)=nums(i)+presum(i-1)$ 。

我们用 $minPresum(i)$ 表示前 $i$ 个前缀和中最小的一个，即 $minPresum(i)=min_{0\leq i\leq n-1}presum(i)$ 。

我们用 $f(i)$ 表示以第 $i$ 的数结尾的 _和最大连续子数组_ ，由以上两个我们可以得出，$f(i)=max\{presum(i)-minPresum(i)\}$ 。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int[] presum = new int[n + 1];
        int minPresum = 0;
        int res = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            presum[i + 1] = presum[i] + nums[i];
            res = Math.max(res, presum[i + 1] - minPresum);
            minPresum = Math.min(presum[i + 1], minPresum);
        }
        return res;
    }
}
```

由于 $presum(i)$ 只与 $nums(i)$ 有关，所以我们可以省略 $presum(i)$ 的过程值。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int presum = 0;
        int res = Integer.MIN_VALUE;
        int minPresum = 0;
        for (int i : nums) {
            presum += i;
            res = Math.max(res, presum - minPresum);
            minPresum = Math.min(minPresum, presum);
        }
        return res;
    }
}
