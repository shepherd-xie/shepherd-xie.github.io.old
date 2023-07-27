---
title: leetcode 2595. 奇偶位数
date: 2023-06-27 14:48:53
tags:
  - 位运算
  - easy
  - leetcode
categories:
  - 算法
top:
---

> [2595. 奇偶位数](https://leetcode.cn/problems/number-of-even-and-odd-bits/description/)
>

<!-- more -->

> 给你一个 **正** 整数 `n` 。
>
> 用 `even` 表示在 `n` 的二进制形式（下标从 **0** 开始）中值为 `1` 的偶数下标的个数。
>
> 用 `odd` 表示在 `n` 的二进制形式（下标从 **0** 开始）中值为 `1` 的奇数下标的个数。
>
> 返回整数数组 `answer` ，其中 `answer = [even, odd]` 。
>
>  
>
> **示例 1：**
>
>  ```
> 输入：n = 17
> 输出：[2,0]
> 解释：17 的二进制形式是 10001 。 
> 下标 0 和 下标 4 对应的值为 1 。 
> 共有 2 个偶数下标，0 个奇数下标。
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 2
> 输出：[0,1]
> 解释：2 的二进制形式是 10 。 
> 下标 1 对应的值为 1 。 
> 共有 0 个偶数下标，1 个奇数下标。
> ```
>
> 
>
> **提示：**
> 
>  - `1 <= n <= 1000`

确实是简单题，像我这种菜鸡都能很快的理清思路，显然是利用 *移位* 操作和位运算来解决。直接上题解吧。

```java
public int[] evenOddBit(int n) {
    int[] result = new int[2];
    int count = 0;
    while (n != 0) {
        if ((n & 1) == 1) {
            result[count] += 1;
        }
        count ^= 1;
        n = n >>> 1;
    }
    return result;
}
```

再看看大神们的操作，显然比我的方法简洁太多。

```java
public int[] evenOddBit(int n) {
    var ans = new int[2];
    for (int i = 0; n > 0; i ^= 1, n >>= 1)
        ans[i] += n & 1;
    return ans;
}
```

当然，另一个操作就更神奇了，我们完全可以利用掩码的方式将原始字符做处理。

>```java
> public int[] evenOddBit(int n) {
>     final int MASK = 0x5555;
>     return new int[]{Integer.bitCount(n & MASK), Integer.bitCount(n & (MASK >> 1))};
> }
>```
>
>作者：灵茶山艾府
>链接：https://leetcode.cn/problems/number-of-even-and-odd-bits/solutions/2177848/er-jin-zhi-ji-ben-cao-zuo-pythonjavacgo-o82o2/
>来源：力扣（LeetCode）
