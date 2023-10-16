---
title: 136. 只出现一次的数字
date: 2023-10-16 14:44:02
tags:
  - 位运算
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/single-number/description/

<!-- more -->

> 给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>
> 你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。
>
>  
>
> **示例 1 ：**
>
> ```
> 输入：nums = [2,2,1]
> 输出：1
> ```
>
> **示例 2 ：**
>
> ```
> 输入：nums = [4,1,2,1,2]
> 输出：4
> ```
>
> **示例 3 ：**
>
> ```
> 输入：nums = [1]
> 输出：1
> ```
>
>  
>
> **提示：**
>
> - `1 <= nums.length <= 3 * 104`
> - `-3 * 104 <= nums[i] <= 3 * 104`
> - 除了某个元素只出现一次以外，其余每个元素均出现两次。

由于异或的特殊性质 `A ^ B ^ B = A` 所以可以将整个数组以此异或，最后得到的就是出现一次的数。

```java
class Solution {
    public int singleNumber(int[] nums) {
        int res = 0;
        for (int num : nums) {
            res ^= num;
        }
        return res;
    }
}
```
