---
title: 316. 去除重复字母
date: 2023-07-20 11:13:00
tags:
  - 单调栈
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/remove-duplicate-letters/description/

<!-- more -->

> 给你一个字符串 `s` ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 **返回结果的字典序最小**（要求不能打乱其他字符的相对位置）。
>
>  
>
>  **示例 1：**
>
> ```
>输入：s = "bcabc"
> 输出："abc"
>```
> 
> **示例 2：**
> 
> ```
> 输入：s = "cbacdcbc"
>输出："acdb"
> ```
>
>  
>
> **提示：**
> 
> - `1 <= s.length <= 104`
> - `s` 由小写英文字母组成

通常意义上说的，字越少，事越大，就是在说这道题。看上去题目很短，给的信息也不多，比较关键的几个点 `去重` `字典序` `由小写字母组成` 。

* 首先我们准备一个栈，然后遍历字符串
* 如果这个字符在栈中出现过，那么跳过本次操作进行下一次遍历
* 当前字符没有在栈中出现过，那么判断当前字符与栈顶元素的关系
  * 如果栈顶元素字典序大于当前元素，且栈顶元素在后面还会出现，则弹出栈顶
  * 再次判断，直到栈顶元素字典序小于当前元素，或栈顶元素不会再出现，或栈空
* 压入当前元素
* 最后输出栈内元素即可

```java
class Solution {
    public static String removeDuplicateLetters(String s) {
        int[] hash = new int[26];
        // 记录每个元素最后一次出现的位置
        for (int i = 0; i < s.length(); i++) {
            hash[s.charAt(i) - 'a'] = i;
        }
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            if (stack.contains(s.charAt(i))) {
                continue;
            }

            while (!stack.isEmpty() && stack.peek() > s.charAt(i) && hash[stack.peek() - 'a'] > i) {
                stack.pop();
            }

            stack.push(s.charAt(i));
        }
        
        StringBuilder sb = new StringBuilder();
        for (Character c : stack) {
            sb.append(c);
        }
        return sb.toString();
    }
}
```

