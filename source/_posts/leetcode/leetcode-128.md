---
title: 128. 最长连续序列
date: 2023-07-14 14:35:13
tags:
  - 哈希表
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/longest-consecutive-sequence/description/

<!-- more -->

> 给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
>
> 请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [100,4,200,1,3,2]
> 输出：4
> 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [0,3,7,2,5,8,4,6,0,1]
> 输出：9
> ```
>
>  
>
> **提示：**
>
> - `0 <= nums.length <= 105`
> - `-109 <= nums[i] <= 109`

因为时间复杂度需要 O(n) 所以就不能用排序了。

初步思路为，遍历每一个数 x ，然后判断 x+1 , x+2, x+3 ... 是否在这个序列中，那么哈希表就最适合这种操作了。

第二个问题，如何防止重复判断，如 1234 和 234 。由于我们一定是从最小的开始向后判断，那么只要 x-1 不在序列中，那么它就一定是连续序列最小的一个。如 [1,2,3,4] ， x=1 时 x-1=0 不在序列中，则 x=1 可以作为连续序列最小， x=2 时 x-1=1 在队列中，则跳过。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for (int num : nums) {
            set.add(num);
        }

        int res = 0;
        for (int num : set) {
            if (set.contains(num - 1)) continue;
            int cur = num;
            // 跳出循环一定是连续序列最大值 +1 因为不在哈希表中
            while (set.contains(cur)) cur ++;
            // 所以连续序列长度为 cur - num
            res = Math.max(res, cur - num);
        }
        return res;
    }
}
```

