---
title: 14. 最长公共前缀
date: 2023-09-13 15:16:02
tags:
  - 字符串
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/longest-common-prefix/description/

<!-- more -->

> 编写一个函数来查找字符串数组中的最长公共前缀。
>
> 如果不存在公共前缀，返回空字符串 `""`。
>
>  
>
> **示例 1：**
>
> ```
> 输入：strs = ["flower","flow","flight"]
> 输出："fl"
> ```
>
> **示例 2：**
>
> ```
> 输入：strs = ["dog","racecar","car"]
> 输出：""
> 解释：输入不存在公共前缀。
> ```
>
>  
>
> **提示：**
>
> - `1 <= strs.length <= 200`
> - `0 <= strs[i].length <= 200`
> - `strs[i]` 仅由小写英文字母组成

1. 因为数组长度大于 1 ,所以以第一个字符串作为 res 去进行比较
2. 从数组第二项开始遍历，与 res 进行比较，将公共前缀作为新的 res 值
   1. 如果没有公共前缀直接返回空字符串
3. 遍历结束返回 res

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        String res = strs[0];
        for (int i = 1; i < strs.length; i ++) {
            int point = 0;
            while (point < res.length() && point < strs[i].length() && res.charAt(point) == strs[i].charAt(point)) {
                point ++;
            }
            if (point == 0) {
                return "";
            }
            res = res.substring(0, point);
        }
        return res;
    }
}
```

