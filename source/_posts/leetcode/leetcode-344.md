---
title: 344. 反转字符串
date: 2023-08-07 11:10:51
tags:
  - 数组
  - easy
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/reverse-string/description/

<!-- more -->

> - 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。
>
>    不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。
>
>    
>
>   **示例 1：**
> 
>  ```
>   输入：s = ["h","e","l","l","o"]
>  输出：["o","l","l","e","h"]
>   ```
>
>   **示例 2：**
> 
>   ```
>   输入：s = ["H","a","n","n","a","h"]
>   输出：["h","a","n","n","a","H"]
>   ```
> 
>    
>
>    **提示：**
>
>   - `1 <= s.length <= 105`
>   - `s[i]` 都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符

题目要求原地修改数组，即我们需要将数组中首位两端的字符两两进行交换，如 `s[0] <=> s[s.length - 1]` `s[1] <=> s[s.length - 2]` `s[i] <=> s[s.length - 1 - i]` 。

我们初始化两个指针，left = 0 和 right = s.length - 1 ，每次交换完成之后 left 自增、right 自减，直到 left >= right 停止交换。

```java
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;
        while (left < right) {
            char temp = s[left];
            s[left ++] = s[right];
            s[right --] = temp;
        }
    }
}
```

