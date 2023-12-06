---
title: 383. 赎金信
date: 2023-12-06 12:52:20
tags:
  - 字符串
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/ransom-note/description/

<!-- more -->

> 给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。
>
> 如果可以，返回 `true` ；否则返回 `false` 。
>
> `magazine` 中的每个字符只能在 `ransomNote` 中使用一次。
>
>   
>
> **示例 1：**
>
> ```
> 输入：ransomNote = "a", magazine = "b"
> 输出：false
> ```
> 
>**示例 2：**
> 
>```
> 输入：ransomNote = "aa", magazine = "ab"
> 输出：false
> ```
> 
> **示例 3：**
>
> ```
>输入：ransomNote = "aa", magazine = "aab"
> 输出：true
> ```
> 
>  
>
>  **提示：**
>
> - `1 <= ransomNote.length, magazine.length <= 105`
>- `ransomNote` 和 `magazine` 由小写英文字母组成

全由小写英文字母组成，简单的统计一下词频即可。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] memo = new int[26];
        for (char c : magazine.toCharArray()) {
            memo[c - 'a']++;
        }
        for (char c : ransomNote.toCharArray()) {
            if (memo[c - 'a'] == 0) {
                return false;
            }
            memo[c - 'a']--;
        }
        return true;
    }
}
```
