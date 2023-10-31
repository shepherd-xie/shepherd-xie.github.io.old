---
title: 392. 判断子序列
date: 2023-10-31 10:14:07
tags:
  - 字符串
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/is-subsequence/description/

<!-- more -->

> 给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。
>
> 字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。
>
> **进阶：**
>
> 如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？
>
> **致谢：**
>
> 特别感谢 [@pbrother ](https://leetcode.com/pbrother/)添加此问题并且创建所有测试用例。
>
>  
>
> **示例 1：**
>
> ```
> 输入：s = "abc", t = "ahbgdc"
> 输出：true
> ```
>
> **示例 2：**
>
> ```
> 输入：s = "axc", t = "ahbgdc"
> 输出：false
> ```
>
>  
>
> **提示：**
>
> - `0 <= s.length <= 100`
> - `0 <= t.length <= 10^4`
> - 两个字符串都只由小写字符组成。

使用双指针 `ps` `pt` 分别指向 `s` `t` ，逐位判断字符是否相同，`pt` 始终向后移动，`ps` 字符与 `pt` 一致时再向后移动，直到字符串尾。

当 `ps` 与 `s` 长度一致时，则表示 `s` 在 `t` 中全部依序出现。

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int ps = 0;
        int pt = 0;
        while (ps < s.length() && pt < t.length()) {
            if (s.charAt(ps) == t.charAt(pt ++)) {
                ps ++;
            }
        }
        return ps == s.length();
    }
}
```

