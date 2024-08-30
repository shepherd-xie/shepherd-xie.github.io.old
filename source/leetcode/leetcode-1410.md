---
title: 1410. HTML 实体解析器
date: 2023-11-23 10:33:09
tags:
  - 字符串
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/html-entity-parser/description/

<!-- more -->

> 「HTML 实体解析器」 是一种特殊的解析器，它将 HTML 代码作为输入，并用字符本身替换掉所有这些特殊的字符实体。
>
> HTML 里这些特殊字符和它们对应的字符实体包括：
>
> - **双引号：**字符实体为 `"` ，对应的字符是 `"` 。
> - **单引号：**字符实体为 `'` ，对应的字符是 `'` 。
> - **与符号：**字符实体为 `&` ，对应对的字符是 `&` 。
> - **大于号：**字符实体为 `>` ，对应的字符是 `>` 。
> - **小于号：**字符实体为 `<` ，对应的字符是 `<` 。
> - **斜线号：**字符实体为 `⁄` ，对应的字符是 `/` 。
>
> 给你输入字符串 `text` ，请你实现一个 HTML 实体解析器，返回解析器解析后的结果。
>
>  
>
> **示例 1：**
>
> ```
> 输入：text = "&amp; is an HTML entity but &ambassador; is not."
> 输出："& is an HTML entity but &ambassador; is not."
> 解释：解析器把字符实体 &amp; 用 & 替换
> ```
>
> **示例 2：**
>
> ```
> 输入：text = "and I quote: &quot;...&quot;"
> 输出："and I quote: \"...\""
> ```
>
> **示例 3：**
>
> ```
> 输入：text = "Stay home! Practice on Leetcode :)"
> 输出："Stay home! Practice on Leetcode :)"
> ```
>
> **示例 4：**
>
> ```
> 输入：text = "x &gt; y &amp;&amp; x &lt; y is always false"
> 输出："x > y && x < y is always false"
> ```
>
> **示例 5：**
>
> ```
> 输入：text = "leetcode.com&frasl;problemset&frasl;all"
> 输出："leetcode.com/problemset/all"
> ```
>
>  
>
> **提示：**
>
> - `1 <= text.length <= 10^5`
> - 字符串可能包含 256 个ASCII 字符中的任意字符。

这种问题直接调标准库，不过要注意将 `$amp;` 的替换放到最后。

```java
class Solution {
    public String entityParser(String text) {
        return text
                .replaceAll("&quot;", "\"")
                .replaceAll("&apos;", "'")
                .replaceAll("&gt;", ">")
                .replaceAll("&lt;", "<")
                .replaceAll("&frasl;", "/")
                .replaceAll("&amp;", "&");
    }
}
```

当然，正常的流程还是要走的。这里可以使用双指针，当慢指针指向 `&` 时，快指针寻找下一个 `;` ，然后去哈希表寻找对应的字符串进行拼装替换。需要额外注意连续多个起始字符或没有终止字符的情况，如：`&&&`。

```java
class Solution {
    public String entityParser(String text) {
        Map<String, String> dict = new HashMap<>();
        dict.put("&quot;", "\"");
        dict.put("&apos;", "'");
        dict.put("&amp;", "&");
        dict.put("&gt;", ">");
        dict.put("&lt;", "<");
        dict.put("&frasl;", "/");
        
        StringBuilder sb = new StringBuilder();
        int n = text.length();
        int i = 0;
        while (i < n) {
            if ('&' != text.charAt(i)) {
                sb.append(text.charAt(i));
            } else {
                int j = i;
                while (j < n && ';' != text.charAt(j)) j++;
                if (j == n) {
                    sb.append(text.charAt(i));
                } else {
                    String originStr = text.substring(i, j + 1);
                    String s = dict.get(originStr);
                    if (s != null) {
                        sb.append(s);
                        i = j;
                    } else {
                        sb.append(text.charAt(i));
                    }
                }
            }
            i++;
        }
        return sb.toString();
    }
}
```

