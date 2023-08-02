---
title: 771. 宝石与石头
date: 2023-07-24 10:53:24
tags:
  - 哈希表
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/jewels-and-stones/description/

<!-- more -->

> 给你一个字符串 `jewels` 代表石头中宝石的类型，另有一个字符串 `stones` 代表你拥有的石头。 `stones` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。
>
> 字母区分大小写，因此 `"a"` 和 `"A"` 是不同类型的石头。
>
>  
>
> **示例 1：**
>
> ```
> 输入：jewels = "aA", stones = "aAAbbbb"
> 输出：3
> ```
>
> **示例 2：**
>
> ```
> 输入：jewels = "z", stones = "ZZ"
> 输出：0
> ```
>
>  
>
> **提示：**
>
> - `1 <= jewels.length, stones.length <= 50`
> - `jewels` 和 `stones` 仅由英文字母组成
> - `jewels` 中的所有字符都是 **唯一的**

## 解法一：哈希表

```java
class Solution {
    public int numJewelsInStones(String jewels, String stones) {
        Set<Character> hashset = new HashSet<>();
        int res = 0;
        for (char c : jewels.toCharArray()) {
            hashset.add(c);
        }
        for (char c : stones.toCharArray()) {
            if (hashset.contains(c)) {
                res ++;
            }
        }
        return res;
    }
}
```

## 解法二：位运算

观察 ASCII 表，可以发现：

* 大写字母二进制的低 666 位是从 000001000001000001 开始的（对应大写字母 A），一直到 011010011010011010（对应大写字母 Z）。
* 小写字母二进制的低 666 位是从 100001100001100001 开始的（对应小写字母 a），一直到 111010111010111010（对应小写字母 z），即十进制的 58。

由于最大位数不超过 58 ，那么我们可以将其压缩存储在一个 64 位长度的数据结构上。

```java
class Solution {
    public int numJewelsInStones(String jewels, String stones) {
        long mask = 0;
        for (char c : jewels.toCharArray()) {
            mask |= 1L << (c & 63);
        }
        int res = 0;
        for (char c : stones.toCharArray()) {
            res += (mask >> (c & 63)) & 1; 
        }
        return res;
    }
}
```

