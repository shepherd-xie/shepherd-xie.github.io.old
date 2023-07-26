---
title: 454. 四数相加 II
date: 2023-07-03 13:47:35
tags:
  - 哈希表
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

> [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/description/)
>
> 
>
> 给你四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` ，数组长度都是 `n` ，请你计算有多少个元组 `(i, j, k, l)` 能满足：
>
> - `0 <= i, j, k, l < n`
> - `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`
>
> 
>
> **示例 1：**
>
> ```
> 输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
> 输出：2
> 解释：
>  两个元组如下：
> 1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
> 2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
> ```
>
> **示例 2：**
>
> ```
> 输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
> 输出：1
> ```
>
> 
>
>  **提示：**
>
> - `n == nums1.length`
> - `n == nums2.length`
> - `n == nums3.length`
> - `n == nums4.length`
> - `1 <= n <= 200`
> - `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`

像这种可以计算的题，最简单直接的思路便是多重循环。

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int result = 0;
        for (int i = 0; i < nums1.length; i ++) {
            for (int j = 0; j < nums2.length; j ++) {
                for (int k = 0; k < nums3.length; k ++) {
                    for (int l = 0; l < nums4.length; l++) {
                        if (nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0) {
                            result ++;
                        }
                    }
                }
            }
        }
        return result;
    }
}
```

超不超时什么的是管不了的，起码这是一种行之有效的方法。当然，对于我们来说一定是为了去寻求更加有效的方法。

审查题目后发现一个很有意思的点 **和为0**，众所周知，在数学的世界里，对于0、1、-1这样的数字都要相当敏感。

当0出现时，`nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`就可以变为 `nums1[i] + nums2[j] = nums3[k] + nums4[l]` ，即，我们从最初的四重循环，降低到了两重循环，在时间复杂度上就减少了两个量级。

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num1 : nums1) {
            for (int num2 : nums2) {
                map.put(num1 + num2, map.getOrDefault(num1 + num2, 0) + 1);
            }
        }
        int result = 0;
        for (int num3 : nums3) {
            for (int num4 : nums4) {
                result += map.getOrDefault(-num3 - num4, 0);
            }
        }
        return result;
    }
}
```
