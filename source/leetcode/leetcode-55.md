---
title: 55. 跳跃游戏
date: 2023-10-07 11:21:03
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/jump-game/description/

<!-- more -->

>给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。
>
>判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。
>
> 
>
>**示例 1：**
>
>```
>输入：nums = [2,3,1,1,4]
>输出：true
> 解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
>```
>
>**示例 2：**
>
>```
>输入：nums = [3,2,1,0,4]
>输出：false
>解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
>```
>
> 
>
>**提示：**
>
>- `1 <= nums.length <= 104`
>- `0 <= nums[i] <= 105`

我们定义一个 max 用来记录当前所能到达的最大下标，初始化为 0 ，遍历数组，当数组下标大于 max 时则表示当前下标大于能到达的最大下标，返回 false 。可以访问改下标时，更新 max 为当前下标所能到达的最远距离与 max 的最大值。

```java
class Solution {
    public boolean canJump(int[] nums) {
        int max = 0;
        for (int i = 0; i < nums.length && i <= max; i ++) {
            max = Math.max(max, i + nums[i]);
        }
        return max >= nums.length - 1;
    }
}
```

