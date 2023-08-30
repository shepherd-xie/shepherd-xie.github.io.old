---
title: 66. 加一
date: 2023-08-30 15:10:33
tags:
  - 数组
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/plus-one/description/

<!-- more -->

> 给定一个由 **整数** 组成的 **非空** 数组所表示的非负整数，在该数的基础上加一。
>
> 最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。
>
> 你可以假设除了整数 0 之外，这个整数不会以零开头。
>
>  
>
> **示例 1：**
>
> ```
> 输入：digits = [1,2,3]
> 输出：[1,2,4]
> 解释：输入数组表示数字 123。
> ```
>
> **示例 2：**
>
> ```
> 输入：digits = [4,3,2,1]
> 输出：[4,3,2,2]
> 解释：输入数组表示数字 4321。
> ```
>
> **示例 3：**
>
> ```
> 输入：digits = [0]
> 输出：[1]
> ```
>
>  
>
> **提示：**
>
> - `1 <= digits.length <= 100`
> - `0 <= digits[i] <= 9`

### 方法一

既然是加一，那么按照正常的加法加一即可，不过由于需要考虑进位，那么在当前位数为 9   的时候就要置为 0 然后对下一位进行处理。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i --) {
            if (digits[i] != 9) {
                digits[i] += 1;
                return digits;
            }
            digits[i] = 0;
        }
        int[] res = new int[digits.length + 1];
        res[0] = 1;
        return res;
    }
}
```

### 方法二

更容易理解的方法则是将数组转换为 int 型，加一后再转为数组即可。
