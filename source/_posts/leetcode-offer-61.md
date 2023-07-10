---
title: 剑指 Offer 61. 扑克牌中的顺子
date: 2023-07-05 10:49:11
tags:
  - 数组
  - easy
  - leetcode
categories:
  - 算法
top:
---

> [剑指 Offer 61. 扑克牌中的顺子](https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/description/)
>
> 
>
> 从**若干副扑克牌**中随机抽 `5` 张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
>
>  
> 
>**示例 1:**
> 
>```
> 输入: [1,2,3,4,5]
>输出: True
> ```
> 
>  
> 
> **示例 2:**
> 
> ```
> 输入: [0,0,1,2,5]
>输出: True
> ```
>
>  
> 
> **限制：**
> 
>数组长度为 5 
> 
>数组的数取值为 [0, 13] .

最直接能够想到的方法就是先进行排序，然后遍历，获取 0 的数量后，判断后续差值，若差值大于王的数量则不连续。

```java
class Solution {
    public boolean isStraight(int[] nums) {
        Arrays.sort(nums);
        int kings = 0;
        int i = 0;
        for (int num : nums) {
            if (num == 0) {
                kings ++;
                i ++;
            }
        }
        for (i ++; i < nums.length; i++) {
            if (nums[i] - nums[i - 1] > 1) {
                if (kings == 0) {
                    return false;
                }
                if (nums[i] - nums[i - 1] - 1 <= kings) {
                    kings -= nums[i] - nums[i - 1] - 1;
                } else {
                    return false;
                }
            } else if (nums[i] == nums[i - 1]) {
                return false;
            }
        }
        return true;
    }
}
```

其实在当前题目的规则之下，只要判断除 0 外的数字中，最大值减去最小值只要小于 5 那么一定能够组成顺子。

```java
class Solution {
    public boolean isStraight(int[] nums) {
        int joker = 0;
        Arrays.sort(nums); // 数组排序
        for(int i = 0; i < 4; i++) {
            if(nums[i] == 0) joker++; // 统计大小王数量
            else if(nums[i] == nums[i + 1]) return false; // 若有重复，提前返回 false
        }
        return nums[4] - nums[joker] < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
    }
}

作者：Krahets
链接：https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/solutions/212071/mian-shi-ti-61-bu-ke-pai-zhong-de-shun-zi-ji-he-se/
来源：力扣（LeetCode）
```

