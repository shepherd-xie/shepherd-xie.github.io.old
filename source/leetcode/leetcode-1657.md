---
title: 1657. 确定两个字符串是否接近
date: 2023-11-30 09:41:30
tags:
  - 字符串
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/determine-if-two-strings-are-close/description/

<!-- more -->

> 如果可以使用以下操作从一个字符串得到另一个字符串，则认为两个字符串 **接近** ：
>
> - 操作 1：交换任意两个 **现有** 字符
>   - 例如，`abcde -> aecdb`
> - 操作 2：将一个 **现有** 字符的每次出现转换为另一个 **现有** 字符，并对另一个字符执行相同的操作。
>   - 例如，`aacabb -> bbcbaa`（所有 `a` 转化为 `b` ，而所有的 `b` 转换为 `a` ）
>
> 你可以根据需要对任意一个字符串多次使用这两种操作。
>
> 给你两个字符串，`word1` 和 `word2` 。如果 `word1` 和 `word2` **接近** ，就返回 `true` ；否则，返回 `false` 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：word1 = "abc", word2 = "bca"
> 输出：true
> 解释：2 次操作从 word1 获得 word2 。
> 执行操作 1："abc" -> "acb"
> 执行操作 1："acb" -> "bca"
> ```
>
> **示例 2：**
>
> ```
> 输入：word1 = "a", word2 = "aa"
> 输出：false
> 解释：不管执行多少次操作，都无法从 word1 得到 word2 ，反之亦然。
> ```
>
> **示例 3：**
>
> ```
> 输入：word1 = "cabbba", word2 = "abbccc"
> 输出：true
> 解释：3 次操作从 word1 获得 word2 。
> 执行操作 1："cabbba" -> "caabbb"
> 执行操作 2："caabbb" -> "baaccc"
> 执行操作 2："baaccc" -> "abbccc"
> ```
>
> **示例 4：**
>
> ```
> 输入：word1 = "cabbba", word2 = "aabbss"
> 输出：false
> 解释：不管执行多少次操作，都无法从 word1 得到 word2 ，反之亦然。
> ```
>
>  
>
> **提示：**
>
> - `1 <= word1.length, word2.length <= 105`
> - `word1` 和 `word2` 仅包含小写英文字母

如果两个字符串接近必须满足以下条件：

* 长度相同
* 出现的字符相同
* 出现的字符频率相同

```java
class Solution {
    public boolean closeStrings(String word1, String word2) {
        if (word1.length() != word2.length()) {
            return false;
        }
        int n = word1.length();
        int[] memo1 = new int[26];
        int[] memo2 = new int[26];
        // 统计字符频率
        for (int i = 0; i < n; i++) {
            memo1[word1.charAt(i) - 'a']++;
            memo2[word2.charAt(i) - 'a']++;
        }
        for (int i = 0; i < memo1.length; i++) {
            // 当两个字符串构成字符不同时直接返回
            if ((memo1[i] != 0) != (memo2[i] != 0)) {
                return false;
            }
            if (memo1[i] != 0) {
                boolean flag = true;
                // 判断字符频率
                for (int j = 0; j < memo2.length && flag; j++) {
                    // 统计过的字符频率设置为-1，不设置0防止在判断字符构成时误判
                    if (memo1[i] == memo2[j]) {
                        memo2[j] = -1;
                        flag = false;
                    }
                }
                if (flag) {
                    return false;
                }
            }
        }
        return true;
    }
}
```
