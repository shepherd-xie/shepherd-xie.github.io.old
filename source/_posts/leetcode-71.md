---
title: 71. 简化路径
date: 2023-06-30 14:13:29
tags:
  - 字符串
  - 栈
  - medium
  - leetcode
categories:
  - 算法
top:
---

> [71. 简化路径](https://leetcode.cn/problems/simplify-path/description/)
>
> 
>
> 给你一个字符串 `path` ，表示指向某一文件或目录的 Unix 风格 **绝对路径** （以 `'/'` 开头），请你将其转化为更加简洁的规范路径。
>
> 在 Unix 风格的文件系统中，一个点（`.`）表示当前目录本身；此外，两个点 （`..`） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。任意多个连续的斜杠（即，`'//'`）都被视为单个斜杠 `'/'` 。 对于此问题，任何其他格式的点（例如，`'...'`）均被视为文件/目录名称。
>
> 请注意，返回的 **规范路径** 必须遵循下述格式：
>
> - 始终以斜杠 `'/'` 开头。
> - 两个目录名之间必须只有一个斜杠 `'/'` 。
> - 最后一个目录名（如果存在）**不能** 以 `'/'` 结尾。
> - 此外，路径仅包含从根目录到目标文件或目录的路径上的目录（即，不含 `'.'` 或 `'..'`）。
>
> 返回简化后得到的 **规范路径** 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：path = "/home/"
> 输出："/home"
> 解释：注意，最后一个目录名后面没有斜杠。 
> ```
>
> **示例 2：**
>
> ```
> 输入：path = "/../"
> 输出："/"
> 解释：从根目录向上一级是不可行的，因为根目录是你可以到达的最高级。
> ```
>
> **示例 3：**
>
> ```
> 输入：path = "/home//foo/"
> 输出："/home/foo"
> 解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。
> ```
>
> **示例 4：**
>
> ```
> 输入：path = "/a/./b/../../c/"
> 输出："/c"
> ```
>
>  
>
> **提示：**
>
> - `1 <= path.length <= 3000`
> - `path` 由英文字母，数字，`'.'`，`'/'` 或 `'_'` 组成。
> - `path` 是一个有效的 Unix 风格绝对路径。

虽然是一道中等难度的题，但是不难看出还是比较简单的，只要慢慢理清思路即可。

* 第一步，将字符串按照斜杠分割成数组。
* 第二步，遍历数组判断当前路径字符串组成
  * 若为空或是 `.` 则跳过不处理
  * 若为 `..` 则删除上一个保存的路径
  * 不为以上两种则保存至待栈中
* 第三步，组合栈中的路径，若栈为空则输出根路径。

```java
class Solution {
    public String simplifyPath(String path) {
        String[] split = path.split("/");
        LinkedList<String> list = new LinkedList<>();
        for (String s : split) {
            if (".".equals(s) || s.isEmpty()) {
                continue;
            }
            if ("..".equals(s)) {
                if (!list.isEmpty()) {
                    list.removeLast();
                }
                continue;
            }
            list.add(s);
        }
        StringBuilder sb = new StringBuilder();
        for (String s : list) {
            sb.append('/').append(s);
        }
        return list.isEmpty() ? "/" : sb.toString();
    }
}

```

