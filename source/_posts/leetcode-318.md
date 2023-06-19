---
title: leetcode-318 最大单词长度乘积
date: 2023-06-16 14:40:35
tags:
  - 位运算
  - 字符串
categories:
  - leetcode
  - 算法
top:
---

> [318. 最大单词长度乘积](https://leetcode.cn/problems/maximum-product-of-word-lengths/)
>
> 
>
> - 给你一个字符串数组 `words` ，找出并返回 `length(words[i]) * length(words[j])` 的最大值，并且这两个单词不含有公共字母。如果不存在这样的两个单词，返回 `0` 。
>
>    
>
>   **示例 1：**
>
>   ```
>  输入：words = ["abcw","baz","foo","bar","xtfn","abcdef"]
>   输出：16 
>  解释：这两个单词为 "abcw", "xtfn"。
>   ```
> 
>   **示例 2：**
> 
>   ```
>   输入：words = ["a","ab","abc","d","cd","bcd","abcd"]
>   输出：4 
>   解释：这两个单词为 "ab", "cd"。
>   ```
> 
>   **示例 3：**
> 
>   ```
>  输入：words = ["a","aa","aaa","aaaa"]
>   输出：0 
>  解释：不存在这样的两个单词。
>   ```
> 
>    
> 
>  **提示：**
> 
>  - `2 <= words.length <= 1000`
>   - `1 <= words[i].length <= 1000`
>   - `words[i]` 仅包含小写字母

由题目和提示信息可以归纳出两点：

* 第一，本题难点在于判断两个单词中是否含有公共字母
* 第二，`words[i]` 仅包含小写字母，那便意味着 `words[i].length() = 26` 是一个有限的集合

既然是一个有限的集合那么我们就可以利用第二的特点来解决第一个问题。

我们可以定义一个长度为 26 的数组来代表字符串

> 例如：abcw = [
>
> ​	"1","1","1","0","0","0","0","0","0","0",
>
> ​	"0","0","0","0","0","0","0","0","0","0",
>
> ​	"0","0","1","0","0","0"
>
> ]

这样在进行比较的时候只要按位比较数组上的相同的位置是都存在值即可。

但是已经走到了这一步为什么不用 _位符号_ 来表示字母。

> 例如：abcw = 11100000000000000000001000

在进行比较时直接进行 & 运算即可。

```java
public int maxProduct(String[] words) {
    int[] bitWords = new int[words.length];
    // 对 words 中的数据做位转换处理
    for (int i = 0; i < words.length; i++) {
        char[] chars = words[i].toCharArray();
        for (char aChar : chars) {
            bitWords[i] |= 1 << (aChar - 'a');
        }
    }
    int answer = 0;
    for (int i = 0; i < words.length; i++) {
        for (int j = i + 1; j < words.length; j++) {
            if ((bitWords[i] & bitWords[j]) == 0) {
                answer = Math.max(answer, words[i].length() * words[j].length());
            }
        }
    }
    return answer;
}
```

