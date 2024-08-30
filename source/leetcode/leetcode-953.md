---
title: 953. 验证外星语词典
date: 2023-07-04 12:52:58
tags:
  - 哈希表
  - 字符串
  - easy
  - leetcode
categories:
  - 算法
top:
---

> [953. 验证外星语词典](https://leetcode.cn/problems/verifying-an-alien-dictionary/)
>

<!-- more -->

> 某种外星语也使用英文小写字母，但可能顺序 `order` 不同。字母表的顺序（`order`）是一些小写字母的排列。
>
> 给定一组用外星语书写的单词 `words`，以及其字母表的顺序 `order`，只有当给定的单词在这种外星语中按字典序排列时，返回 `true`；否则，返回 `false`。
>
> 
>
> **示例 1：**
>
>  ```
>输入：words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
> 输出：true
>解释：在该语言的字母表中，'h' 位于 'l' 之前，所以单词序列是按字典序排列的。
> ```
> 
> **示例 2：**
> 
> ```
>输入：words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
> 输出：false
>解释：在该语言的字母表中，'d' 位于 'l' 之后，那么 words[0] > words[1]，因此单词序列不是按字典序排列的。
> ```
> 
> **示例 3：**
> 
> ```
>输入：words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
> 输出：false
>解释：当前三个字符 "app" 匹配时，第二个字符串相对短一些，然后根据词典编纂规则 "apple" > "app"，因为 'l' > '∅'，其中 '∅' 是空白字符，定义为比任何其他字符都小（更多信息）。
> ```
> 
> 
> 
> **提示：**
>
>  - `1 <= words.length <= 100`
>- `1 <= words[i].length <= 20`
> - `order.length == 26`
>- 在 `words[i]` 和 `order` 中的所有字符都是英文小写字母。

判断是否排序就是按照字典顺序比较大小，从最简单的开始判断。如何比较 `a` 和 `b` 的大小，从字典中找到 `a` 和 `b` ，然后再判断他们的字典顺序即可。

那我们可以得到一个简单的思路，在对两个字符串进行比较时，判断同位置的字符字典顺序：

* 如果 `dict{word[i-1]} > dict{word[i]}` ，直接返回 `false` 。
* 如果 `dict{word[i-1]} < dict{word[i]}` ，那么跳过后续判断，进行下一组比较。
* 如果 `dict{word[i-1]} = dict{word[i]}` ，就进行后续字符串的比较。
  * 当后续比较完毕且没有返回或者跳过时，进行字符串长度的比较。

```java
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        if (words.length <= 1) {
            return true;
        }
        int[] dict = new int[order.length()];
        for (int i = 0; i < order.length(); i++) {
            dict[order.charAt(i) - 'a'] = i;
        }

        for (int i = 1; i < words.length; i++) {
            boolean compareLength = true;
            for (int j = 0; j < Math.min(words[i - 1].length(), words[i].length()); j++) {
                if (dict[words[i - 1].charAt(j)  - 'a'] < dict[words[i].charAt(j)  - 'a']) {
                    compareLength = false;
                    break;
                } else if ((dict[words[i - 1].charAt(j)  - 'a'] > dict[words[i].charAt(j)  - 'a'])) {
                    return false;
                }
            }

            if (compareLength && words[i - 1].length() > words[i].length()) {
                return false;
            }
        }
        return true;
    }
}

```

