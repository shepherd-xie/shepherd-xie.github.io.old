---
title: 45. 跳跃游戏 II
date: 2023-10-10 16:43:27
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/jump-game-ii/description/

<!-- more -->

> 给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。
>
> 每个元素 `nums[i]` 表示从索引 `i` 向前跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:
>
>  - `0 <= j <= nums[i]` 
>- `i + j < n`
> 
>返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。
> 
>  
> 
> **示例 1:**
> 
> ```
> 输入: nums = [2,3,1,1,4]
> 输出: 2
> 解释: 跳到最后一个位置的最小跳跃数是 2。
>      从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
> ```
> 
> **示例 2:**
>
> ```
>输入: nums = [2,3,0,1,4]
> 输出: 2
> ```
> 
>  
> 
> **提示:**
> 
> - `1 <= nums.length <= 104`
> - `0 <= nums[i] <= 1000`
> - 题目保证可以到达 `nums[n-1]`

在每一次跳跃时都会存在，当前状况下能够跳到的最远位置，记这个位置为 side 。

遍历数组，找到本次跳跃可以调到的最大位置 `pos = Math.max(pos, i + nums[i])` 直到 `i == side` 。

当 `i == side` 时表示应该进行下一次跳跃，`count ++` 。

```java
class Solution {
    public int jump(int[] nums) {
        int count = 0;
        int side = 0;
        int pos = 0;
        for (int i = 0; i < nums.length - 1; i ++) {
            pos = Math.max(pos, i + nums[i]);
            if (i == side) {
                side = pos;
                count ++;
            }
        }
        return count;
    }
}
```

