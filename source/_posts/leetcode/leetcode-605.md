---
title: 605. 种花问题
date: 2023-08-23 14:16:02
tags:
  - 数组
  - easy
  - leetcode
categories:
  - 算法
top:
---

感觉和昨天的题有些许相似。

https://leetcode.cn/problems/can-place-flowers/

<!-- more -->

> 假设有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花不能种植在相邻的地块上，它们会争夺水源，两者都会死去。
>
> 给你一个整数数组 `flowerbed` 表示花坛，由若干 `0` 和 `1` 组成，其中 `0` 表示没种植花，`1` 表示种植了花。另有一个数 `n` ，能否在不打破种植规则的情况下种入 `n` 朵花？能则返回 `true` ，不能则返回 `false` 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：flowerbed = [1,0,0,0,1], n = 1
> 输出：true
> ```
>
> **示例 2：**
>
> ```
> 输入：flowerbed = [1,0,0,0,1], n = 2
> 输出：false
> ```
>
>  
>
> **提示：**
>
> - `1 <= flowerbed.length <= 2 * 10^4`
> - `flowerbed[i]` 为 `0` 或 `1`
> - `flowerbed` 中不存在相邻的两朵花
> - `0 <= n <= flowerbed.length`

简单的说，我们只需要遍历数组，判断当前值和其前后的值是否都为 0 ，如果都为 0 则插下一朵花，防止后续重复判断。最后再对边界特殊值做处理即可。

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        // n 为 0 直接返回 true
        if (n == 0) {
            return true;
        }
        // 如果数组只有一个，则返回这个位置是否为 0
        if (flowerbed.length == 1) {
            return flowerbed[0] == 0;
        }
        int canPlace = 0;
        for (int i = 0; i < flowerbed.length && canPlace < n; i ++) {
            if (i == 0) {
                // 左边界情况
                if (flowerbed[i] == 0 && flowerbed[i + 1] == 0) {
                    flowerbed[i] = 1;
                    canPlace ++;
                }
            } else if (i == flowerbed.length - 1) {
                // 右边界情况
                if (flowerbed[i] == 0 && flowerbed[i - 1] == 0) {
                    flowerbed[i] = 1;
                    canPlace ++;
                }
            } else {
                if (flowerbed[i] == 0 && flowerbed[i - 1] == 0 && flowerbed[i + 1] == 0) {
                    flowerbed[i] = 1;
                    canPlace ++;
                }
            }
        }
        return canPlace >= n;
    }
}
```

