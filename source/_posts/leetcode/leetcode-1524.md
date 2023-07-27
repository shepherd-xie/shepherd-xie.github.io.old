---
title: leetcode 1524. 和为奇数的子数组数目
date: 2023-06-26 14:36:17
tags:
  - 哈希表
  - leetcode
categories:
  - 算法
top:
---

> [1524. 和为奇数的子数组数目](https://leetcode.cn/problems/number-of-sub-arrays-with-odd-sum/)
>

<!-- more -->

> 给你一个整数数组 `arr` 。请你返回和为 **奇数** 的子数组数目。
>
> 由于答案可能会很大，请你将结果对 `10^9 + 7` 取余后返回。
>
> 
>
> **示例 1：**
>
>  ```
>输入：arr = [1,3,5]
> 输出：4
>解释：所有的子数组为 [[1],[1,3],[1,3,5],[3],[3,5],[5]] 。
> 所有子数组的和为 [1,4,9,3,8,5].
> 奇数和包括 [1,9,3,5] ，所以答案为 4 。
> ```
> 
> **示例 2 ：**
> 
> ```
>输入：arr = [2,4,6]
> 输出：0
>解释：所有子数组为 [[2],[2,4],[2,4,6],[4],[4,6],[6]] 。
> 所有子数组和为 [2,6,12,4,10,6] 。
> 所有子数组和都是偶数，所以答案为 0 。
> ```
> 
> **示例 3：**
> 
> ```
>输入：arr = [1,2,3,4,5,6,7]
> 输出：16
>```
> 
> **示例 4：**
> 
> ```
>输入：arr = [100,100,99,99]
> 输出：4
>```
> 
> **示例 5：**
> 
> ```
>输入：arr = [7]
> 输出：1
>```
> 
> 
> 
> **提示：**
>
>  - `1 <= arr.length <= 10^5`
>- `1 <= arr[i] <= 100`

依照传统思路其实很简单，只需要遍历所有子数组，再求出子数组中各项之和。

不过既然是判断奇偶的操作，那么自然是位运算来的更加方便。

```java
public int numOfSubarrays(int[] arr) {
    int result = 0;
    for (int i = 0; i < arr.length; i++) {
        for (int j = i; j < arr.length; j++) {
            int sum = 0;
            for (int k = i; k <= j; k++) {
                sum ^= (arr[k] & 1);
            }
            if (sum != 0) {
                result ++;
            }
        }
    }
    return result % 1_000_000_007;
}
```

虽然解题思路是对的，但是毕竟是算法题，所以我们有必要去想出更优秀的题解。

可以根据前缀和计算子数组的元素和，并计算和为奇数的子数组数目。

从左到右遍历数组 arr，遍历过程中计算前缀和，并记录偶数前缀和的出现次数 even 与奇数前缀和的出现次数 odd。由于空前缀不包含任何元素，前缀和为 0，因此初始时 even=1，odd=0。

用 *sum* 表示前缀和，初始时 *sum*=0。遍历到每个元素时，执行如下操作。

1. 将当前元素加到 sum。
2. 根据更新后的 sum 计算以当前元素结尾的和为奇数的子数组数目，更新答案。
   - 如果更新后的 sum 是偶数，则以当前元素结尾的和为奇数的子数组数目是 odd，将 odd 加到答案。
   - 如果更新后的 sum 是奇数，则以当前元素结尾的和为奇数的子数组数目是 even，将 even 加到答案。
3. 根据更新后的 sum 的奇偶性，更新偶数前缀和与奇数前缀和的出现次数。
   - 如果更新后的 sum 是偶数，则以当前元素结尾的前缀和是偶数，将 even 加 1。
   - 如果更新后的 sum 是奇数，则以当前元素结尾的前缀和是奇数，将 odd 加 1。

遍历结束之后，即可得到数组 arr 的和为奇数的子数组数目。

> 作者：Storm
> 链接：https://leetcode.cn/problems/number-of-sub-arrays-with-odd-sum/solutions/2254938/1524-he-wei-qi-shu-de-zi-shu-zu-shu-mu-b-p96b/
> 来源：力扣（LeetCode）

```java
public int numOfSubarrays(int[] arr) {
    int MODULO = 1_000_000_007;
    int sum = 0;
    int count = 0;
    int odd = 0;
    int even = 1;
    for (int i = 0; i < arr.length; i++) {
        sum += arr[i];
        if (sum % 2 == 0) {
            count = (count + odd) % MODULO;
            even ++;
        } else {
            count = (count + even) % MODULO;
            odd ++;
        }
    }
    return count;
}
```
