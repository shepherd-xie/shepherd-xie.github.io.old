---
title: 2560. 打家劫舍 IV
date: 2023-09-19 10:47:11
tags:
  - 数组
  - 二分查找
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/house-robber-iv/description/

<!-- more -->

> 沿街有一排连续的房屋。每间房屋内都藏有一定的现金。现在有一位小偷计划从这些房屋中窃取现金。
>
> 由于相邻的房屋装有相互连通的防盗系统，所以小偷 **不会窃取相邻的房屋** 。
>
> 小偷的 **窃取能力** 定义为他在窃取过程中能从单间房屋中窃取的 **最大金额** 。
>
> 给你一个整数数组 `nums` 表示每间房屋存放的现金金额。形式上，从左起第 `i` 间房屋中放有 `nums[i]` 美元。
>
> 另给你一个整数 `k` ，表示窃贼将会窃取的 **最少** 房屋数。小偷总能窃取至少 `k` 间房屋。
>
> 返回小偷的 **最小** 窃取能力。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [2,3,5,9], k = 2
> 输出：5
> 解释：
> 小偷窃取至少 2 间房屋，共有 3 种方式：
> - 窃取下标 0 和 2 处的房屋，窃取能力为 max(nums[0], nums[2]) = 5 。
> - 窃取下标 0 和 3 处的房屋，窃取能力为 max(nums[0], nums[3]) = 9 。
> - 窃取下标 1 和 3 处的房屋，窃取能力为 max(nums[1], nums[3]) = 9 。
> 因此，返回 min(5, 9, 9) = 5 。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [2,7,9,3,1], k = 2
> 输出：2
> 解释：共有 7 种窃取方式。窃取能力最小的情况所对应的方式是窃取下标 0 和 4 处的房屋。返回 max(nums[0], nums[4]) = 2 。
> ```
>
>  
>
> **提示：**
>
> - `1 <= nums.length <= 105`
> - `1 <= nums[i] <= 109`
> - `1 <= k <= (nums.length + 1)/2`

看到「最大化最小值」或者「最小化最大值」就要想到**二分答案**，这是一个固定的套路。

对于 **能偷走的最大金额** 约小，能偷的房间数也就越少，反之亦然。

所以我们使用二分查找方法，去判断对于每一个 **能偷走的最大金额** 所能偷的房间数量与给定的最大盗窃数量作比较。

* 如果盗窃数量大于等于可盗窃数量 `upper = mid - 1`
* 如果盗窃数量小于可盗窃数量 `lower = mid + 1`

对于二分查找结果为什么一定在数组中，可参考 [两种方法：二分+DP/二分+贪心（Python/Java/C++/Go）](https://leetcode.cn/problems/house-robber-iv/solutions/2093952/er-fen-da-an-dp-by-endlesscheng-m558/?envType=daily-question&envId=2023-09-19)

```java
class Solution {
    public int minCapability(int[] nums, int k) {
        int lower = Integer.MAX_VALUE;
        int upper = 0;
        for (int n : nums) {
            lower = Math.min(n, lower);
            upper = Math.max(n, upper);
        }
        // 使用闭区间查找
        while (lower <= upper) {
            int mid = (lower + upper) >>> 1; // 防止 int 溢出
            boolean canrob = true;
            int count = 0;
            for (int n : nums) {
                if (n <= mid && canrob) {
                    count ++;
                    canrob = false;
                } else {
                    canrob = true;
                }
            }
            if (count >= k) {
                upper = mid - 1;
            } else {
                lower = mid + 1;
            }
        }
        return lower;
    }
}
