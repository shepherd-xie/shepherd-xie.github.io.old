---
title: 458. 可怜的小猪
date: 2023-07-28 13:53:52
tags:
  - 数学
  - hard
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/poor-pigs/description/

这道题的评论区充分的展示了某些人就是来地球上充数的，比如我。

<!-- more -->

> - 有` buckets` 桶液体，其中 **正好有一桶** 含有毒药，其余装的都是水。它们从外观看起来都一样。为了弄清楚哪只水桶含有毒药，你可以喂一些猪喝，通过观察猪是否会死进行判断。不幸的是，你只有 `minutesToTest` 分钟时间来确定哪桶液体是有毒的。
>
>   喂猪的规则如下：
>
>    1. 选择若干活猪进行喂养
>  2. 可以允许小猪同时饮用任意数量的桶中的水，并且该过程不需要时间。
>   3. 小猪喝完水后，必须有 `minutesToDie` 分钟的冷却时间。在这段时间里，你只能观察，而不允许继续喂猪。
>  4. 过了 `minutesToDie` 分钟后，所有喝到毒药的猪都会死去，其他所有猪都会活下来。
>   5. 重复这一过程，直到时间用完。
> 
>   给你桶的数目 `buckets` ，`minutesToDie` 和 `minutesToTest` ，返回 *在规定时间内判断哪个桶有毒所需的 **最小** 猪数* 。
> 
>    
> 
>  **示例 1：**
> 
>  ```
>   输入：buckets = 1000, minutesToDie = 15, minutesToTest = 60
>   输出：5
>   ```
> 
>   **示例 2：**
> 
>  ```
>   输入：buckets = 4, minutesToDie = 15, minutesToTest = 15
>  输出：2
>   ```
> 
>   **示例 3：**
> 
>  ```
>   输入：buckets = 4, minutesToDie = 15, minutesToTest = 30
>  输出：2
>   ```
> 
>    
> 
>  **提示：**
>  
>  - `1 <= buckets <= 1000`
>   - `1 <= minutesToDie <= minutesToTest <= 100`

对于两个小猪，喝完毒药`15min` 死去，在`1h`最多能检验多个水桶?答案是`25`桶，如下图所示：

![img](https://images.orkva.com/images/2023/07/28/f57dcc8a-3eb8-4830-97ec-7738301759a5-455593.png)

对于一只猪，可以在`1h`之内最多喝 `4`次水(`60/15`)，但是可以检验5个桶，如果前四次没死，说明第5个桶有毒。

对于2只猪，现在可以让一只猪一下喝`5`桶水，如图所示的一只猪喝**行**的五个，一只猪喝**列**的五个，这样就可以确定哪个桶有毒。

对于3只猪，就是三维的 5 X 5 X 5 ，可以检测125个桶；

> 作者：powcai
> 链接：https://leetcode.cn/problems/poor-pigs/discussion/comments/332116
> 来源：力扣（LeetCode）

```java
class Solution {
    public int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int pigs = 0;
        while (Math.pow((minutesToTest / minutesToDie) + 1, pigs) < buckets)
            pigs ++;
        return pigs;
    }
}
```

当然了，还有更炸裂的思路，这个就自行查阅吧。

> 【宫水三叶】进制猜想 & 香农熵验证
> 作者：宫水三叶
> 链接：https://leetcode.cn/problems/poor-pigs/solutions/1120850/gong-shui-san-xie-jin-zhi-cai-xiang-xian-69fl/
> 来源：力扣（LeetCode）
