---
title: 961. 在长度 2N 的数组中找出重复 N 次的元素
date: 2023-07-10 16:35:39
tags:
  - 哈希表
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/n-repeated-element-in-size-2n-array/

<!-- more -->

> 给你一个整数数组 `nums` ，该数组具有以下属性：
>
> - `nums.length == 2 * n`.
>- `nums` 包含 `n + 1` 个 **不同的** 元素
> - `nums` 中恰有一个元素重复 `n` 次
>
> 找出并返回重复了 `n` 次的那个元素。
> 
> 
>
> **示例 1：**
>
>  ```
>输入：nums = [1,2,3,3]
> 输出：3
>```
> 
> **示例 2：**
> 
> ```
>输入：nums = [2,1,2,5,3,2]
> 输出：2
>```
> 
> **示例 3：**
> 
> ```
>输入：nums = [5,1,5,2,5,3,5,4]
> 输出：5
>```
> 
> 
> 
> **提示：**
>
>  - `2 <= n <= 5000`
>- `nums.length == 2 * n`
> - `0 <= nums[i] <= 104`
>- `nums` 由 `n + 1` 个 **不同的** 元素组成，且其中一个元素恰好重复 `n` 次

这道题过于简单了，就不过多赘述，直接哈希表处理即可。

```java
class Solution {
    public int repeatedNTimes(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            if (map.containsKey(num)) {
                return num;
            }
            map.put(num, 0);
        }
        return -1;
    }
}
```

当然题解区也有很多有意思的思路，值得自己去看看。
