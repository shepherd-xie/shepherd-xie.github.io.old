---
title: 2609. 最长平衡子字符串
date: 2023-11-08 14:49:58
tags:
  - 模拟
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/find-the-longest-balanced-substring-of-a-binary-string/description/

<!-- more -->

> 给你一个仅由 `0` 和 `1` 组成的二进制字符串 `s` 。 
>
> 如果子字符串中 **所有的** `0` **都在** `1` **之前** 且其中 `0` 的数量等于 `1` 的数量，则认为 `s` 的这个子字符串是平衡子字符串。请注意，空子字符串也视作平衡子字符串。 
>
> 返回 `s` 中最长的平衡子字符串长度。
>
> 子字符串是字符串中的一个连续字符序列。
>
>  
>
> **示例 1：**
>
> ```
> 输入：s = "01000111"
> 输出：6
> 解释：最长的平衡子字符串是 "000111" ，长度为 6 。
> ```
>
> **示例 2：**
>
> ```
> 输入：s = "00111"
> 输出：4
> 解释：最长的平衡子字符串是 "0011" ，长度为  4 。
> ```
>
> **示例 3：**
>
> ```
> 输入：s = "111"
> 输出：0
> 解释：除了空子字符串之外不存在其他平衡子字符串，所以答案为 0 。
> ```
>
>  
>
> **提示：**
>
> - `1 <= s.length <= 50`
> - `'0' <= s[i] <= '1'`

直接模拟一下即可：

* 当字符为 0 时
  * 如果之前出现过 1 ，统计平衡子字符串长度，重置 0 和 1 的计数。
  * 不论是否重置过，0 的计数始终 +1.
* 当字符为 1 时
  * 如果 0 的计数为 0 则不对 1 进行计数
* 字符串遍历之后，如果 0 的计数非零则再次统计平衡子字符串长度。

```java
class Solution {
    public int findTheLongestBalancedSubstring(String s) {
        int res = 0;
        int count0 = 0;
        int count1 = 0;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '0') {
                if (count1 != 0) {
                    res = Math.max(res, Math.min(count0, count1) * 2);
                    count0 = 0;
                    count1 = 0;
                }
                count0++;
            } else {
                if (count0 != 0) {
                    count1++;
                }
            }
        }
        if (count0 != 0) {
            res = Math.max(res, Math.min(count0, count1) * 2);
        }
        return res;
    }
}
```

