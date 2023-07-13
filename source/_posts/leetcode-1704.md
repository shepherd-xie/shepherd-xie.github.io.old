---
title: 1704. 判断字符串的两半是否相似
date: 2023-07-12 13:25:33
tags:
  - 字符串
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/determine-if-string-halves-are-alike/description/

<!-- more -->

> 给你一个偶数长度的字符串 `s` 。将其拆分成长度相同的两半，前一半为 `a` ，后一半为 `b` 。
>
> 两个字符串 **相似** 的前提是它们都含有相同数目的元音（`'a'`，`'e'`，`'i'`，`'o'`，`'u'`，`'A'`，`'E'`，`'I'`，`'O'`，`'U'`）。注意，`s` 可能同时含有大写和小写字母。
> 
> 如果 `a` 和 `b` 相似，返回 `true` ；否则，返回 `false` 。
> 
>  
> 
>**示例 1：**
> 
>```
>  输入：s = "book"
>输出：true
>  解释：a = "bo" 且 b = "ok" 。a 中有 1 个元音，b 也有 1 个元音。所以，a 和 b 相似。
>```
> 
>**示例 2：**
> 
>```
> 输入：s = "textbook"
> 输出：false
> 解释：a = "text" 且 b = "book" 。a 中有 1 个元音，b 中有 2 个元音。因此，a 和 b 不相似。
> 注意，元音 o 在 b 中出现两次，记为 2 个。
> ```
> 
> 
> 
>**提示：**
> 
>- `2 <= s.length <= 1000`
> - `s.length` 是偶数
> - `s` 由 **大写和小写** 字母组成

## 解法一：常规思路

分别计算前半段与后半段元音字母个数比较

```java
class Solution {
    public static boolean halvesAreAlike(String s) {
        String str = "aeiouAEIOU";
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            if (str.indexOf(s.charAt(i)) >= 0) {
                if (i < s.length() / 2) {
                    count ++;
                } else {
                    count --;
                }
            }
        }
        return count == 0;
    }
}
```

## 解法二：哈希表

我们可以先将字母全部转为小写，然后利用哈希表的特性只用一次循环就可以解决

```java
class Solution {
    public boolean halvesAreAlike(String s) {
        int[] chars = new int[26];
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (ch <= 'Z') {
                ch += 'a' - 'A';
            }
            if (i < s.length() / 2) {
                chars[ch - 'a'] ++;
            } else {
                chars[ch - 'a'] --;
            }
        }
        return chars[0] + chars['e' - 'a'] + chars['i' - 'a'] + chars['o' - 'a'] + chars['u' - 'a'] == 0;
    }
}
```

