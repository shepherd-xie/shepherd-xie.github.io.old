---
title: 2578. 最小和分割
date: 2023-10-09 14:42:16
tags:
  - 数学
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/split-with-minimum-sum/description/

<!-- more -->

>给你一个正整数 `num` ，请你将它分割成两个非负整数 `num1` 和 `num2` ，满足：
>
>- ```
>  num1
>  ```
>
>   
>
>  和 
>
>  ```
>  num2
>  ```
>
>   直接连起来，得到 
>
>  ```
>  num
>  ```
>
>   各数位的一个排列。
>
>  - 换句话说，`num1` 和 `num2` 中所有数字出现的次数之和等于 `num` 中所有数字出现的次数。
>
>- `num1` 和 `num2` 可以包含前导 0 。
>
>请你返回 `num1` 和 `num2` 可以得到的和的 **最小** 值。
>
>**注意：**
>
>- `num` 保证没有前导 0 。
>- `num1` 和 `num2` 中数位顺序可以与 `num` 中数位顺序不同。
>
> 
>
>**示例 1：**
>
>```
>输入：num = 4325
>输出：59
>解释：我们可以将 4325 分割成 num1 = 24 和 num2 = 35 ，和为 59 ，59 是最小和。
>```
>
>**示例 2：**
>
>```
>输入：num = 687
>输出：75
>解释：我们可以将 687 分割成 num1 = 68 和 num2 = 7 ，和为最优值 75 。
>```
>
> 
>
>**提示：**
>
>- `10 <= num <= 109`

首先题目只要求我们返回最小值，也就是说我们不必去求出两个值相加，而是可以直接求出他们的和。

* 首先将 `num` 拆分为数字数组， `nums[i]` 表示 `i` 在 `num` 中出现过多少次
* 既然要求出最小值，那么拆分出的两个数字一定是尽可能的长度相同，长度最大相差 1。而且数字越小越要占据高位，数字越大越要占据低位。
* 我们将拆分出的数组反向遍历，每遍历两个数字幂数 +1 ，由于前置 0 无效，所以终止条件 > 0 即可

```java
class Solution {
    public int splitNum(int num) {
        int[] nums = new int[10];
        while(num > 0) {
            nums[num % 10] ++;
            num /= 10;
        }
        int power = 1;
        int counter = 0;
        int res = 0;
        for (int i = nums.length - 1; i > 0; ) {
            while (nums[i] > 0) {
                if (counter == 2) {
                    power *= 10;
                    counter = 0;
                }
                res += i * power;
                counter ++;
                nums[i] --;
            }
            i --;
        }
        return res;
    }
}
```

