---
title: 260. 只出现一次的数字 III
date: 2023-10-16 14:46:05
tags:
  - 位运算
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/single-number-iii/description/

<!-- more -->

> 给你一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。你可以按 **任意顺序** 返回答案。
>
> 你必须设计并实现线性时间复杂度的算法且仅使用常量额外空间来解决此问题。
> 
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [1,2,1,3,2,5]
> 输出：[3,5]
>解释：[5, 3] 也是有效的答案。
> ```
>
>  **示例 2：**
>
> ```
>输入：nums = [-1,0]
> 输出：[-1,0]
>```
> 
> **示例 3：**
> 
> ```
>输入：nums = [0,1]
> 输出：[1,0]
>```
> 
> 
> 
> **提示：**
> 
> - `2 <= nums.length <= 3 * 104`
>- `-231 <= nums[i] <= 231 - 1`
>  - 除两个只出现一次的整数外，`nums` 中的其他数字都出现两次

![img](https://images.orkva.com/images/2023/10/16/1697089221-bDdgkc-lc260-c.png)

> 作者：灵茶山艾府
> 链接：https://leetcode.cn/problems/single-number-iii/solutions/2484352/tu-jie-yi-zhang-tu-miao-dong-zhuan-huan-np9d2/ 

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int orxAll = 0;
        for (int num : nums) {
            orxAll ^= num;
        }
        int lowbitMask = orxAll & -orxAll;
        int[] res = new int[2];
        for (int num : nums) {
            res[(num & lowbitMask) == 0 ? 0 : 1] ^= num;
        }
        return res;
    }
}
```

