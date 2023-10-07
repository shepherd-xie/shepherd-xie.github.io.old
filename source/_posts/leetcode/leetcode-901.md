---
title: 901. 股票价格跨度
date: 2023-10-07 11:06:48
tags:
  - 栈
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/online-stock-span/description/

<!-- more -->

>设计一个算法收集某些股票的每日报价，并返回该股票当日价格的 **跨度** 。
>
>当日股票价格的 **跨度** 被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。
>
>- 例如，如果未来 7 天股票的价格是 `[100,80,60,70,60,75,85]`，那么股票跨度将是 `[1,1,1,2,1,4,6]` 。
>
>实现 `StockSpanner` 类：
>
>- `StockSpanner()` 初始化类对象。
>- `int next(int price)` 给出今天的股价 `price` ，返回该股票当日价格的 **跨度** 。
>
> 
>
>**示例：**
>
>```
>输入：
>["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]
>[[], [100], [80], [60], [70], [60], [75], [85]]
>输出：
>[null, 1, 1, 1, 2, 1, 4, 6]
>
>解释：
>StockSpanner stockSpanner = new StockSpanner();
>stockSpanner.next(100); // 返回 1
>stockSpanner.next(80);  // 返回 1
>stockSpanner.next(60);  // 返回 1
>stockSpanner.next(70);  // 返回 2
>stockSpanner.next(60);  // 返回 1
>stockSpanner.next(75);  // 返回 4 ，因为截至今天的最后 4 个股价 (包括今天的股价 75) 都小于或等于今天的股价。
>stockSpanner.next(85);  // 返回 6
>```
>
> 
>
>**提示：**
>
>- `1 <= price <= 105`
>- 最多调用 `next` 方法 `104` 次

类似于此类跨度问题都可以用单调栈来解决，中心思想是维持栈的单调性，及时将无效数据出栈来保持栈的单调。

对于每天的股价，在栈中寻找所有不大于当天的数据并将其出栈，然后计算当前天数与栈顶天数的差值即为当日股价的跨度，然后再将天数据入栈作为新的栈顶。

```java
class StockSpanner {

    Stack<Pair<Integer, Integer>> st;
    int day = 0;

    public StockSpanner() {
        st = new Stack<>();
        st.push(new Pair<>(-1, Integer.MAX_VALUE));
    }
    
    public int next(int price) {
        while (st.peek().getValue() <= price) {
            st.pop();
        }
        int res = day - st.peek().getKey();
        st.push(new Pair<>(day, price));
        day ++;
        return res;
    }
}
```

