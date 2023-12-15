---
title: 28. 找出字符串中第一个匹配项的下标
date: 2023-12-15 11:37:19
tags:
  - 字符串
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/

<!-- more -->

>给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。
>
> 
>
>**示例 1：**
>
>```
>输入：haystack = "sadbutsad", needle = "sad"
>输出：0
>解释："sad" 在下标 0 和 6 处匹配。
>第一个匹配项的下标是 0 ，所以返回 0 。
>```
>
>**示例 2：**
>
>```
>输入：haystack = "leetcode", needle = "leeto"
>输出：-1
>解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
>```
>
> 
>
>**提示：**
>
>- `1 <= haystack.length, needle.length <= 104`
>- `haystack` 和 `needle` 仅由小写英文字符组成

KMP 算法，大一记住了大五又忘了。具体的推荐阮一峰 [字符串匹配的KMP算法](https://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)，实在是不知道怎么讲得清。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int i = 0;
        int j = 0;
        int[] next = getNext(needle);
        while (i < haystack.length() && j < needle.length()) {
            if (j == -1 || haystack.charAt(i) == needle.charAt(j)) {
                i++;
                j++;
            } else {
                j = next[j];
            }
        }
        return j == needle.length() ? i - j : -1;
    }

    public int[] getNext(String str) {
        int[] next = new int[str.length() + 1];
        int i = 0;
        int j = -1;
        next[0] = -1;
        while (i < str.length()) {
            if (j == -1 || str.charAt(i) == str.charAt(j)) {
                i++;
                j++;
                next[i] = j;
            } else {
                j = next[j];
            }
        }
        return next;
    }
}
```

