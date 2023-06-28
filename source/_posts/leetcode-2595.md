---
title: 剑指 Offer 03. 数组中重复的数字
date: 2023-06-28 13:45:21
tags:
  - 数组
  - 哈希表
  - easy
categories:
  - leetcode
  - 算法
top:
---

> [剑指 Offer 03. 数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/description/)
>
> 
>
> 找出数组中重复的数字。
>
> 
>在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>  
>**示例 1：**
> 
>```
>  输入：
> [2, 3, 1, 0, 2, 5, 3]
> 输出：2 或 3 
> ```
> 
>  
> 
>**限制：**
> 
>`2 <= n <= 100000`

这个题目看似是比较简单的，但是可以有多重解法来满足不同的空间复杂度和时间复杂度。

### 方法一：哈希表

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int[] hash = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            if (hash[nums[i]] != 0) {
                return nums[i];
            }
            hash[nums[i]] ++;
        }
        return -1;
    }
}
```

* **时间复杂度**：O(N)
* **空间复杂度**：O(N)

### 方法二：排序

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; i++) {
            if (nums[i - 1] == nums[i]) {
                return nums[i];
            }
        }
        return -1;
    }
}
```

* **时间复杂度**：O(N log(N))，数组排序时间
* **空间复杂度**：O(N)

### 方法三：原地交换

由于题目中指明 `所有数字都在 0～n-1 的范围内`，那么我们可以将数组中的值一一映射至它们对应的下标位置，那么一定会存在同一个下标对应多个相同值得情况。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        for (int i = 0; i < nums.length; ) {
            if (nums[i] == i) {
                i ++;
                continue;
            }
            if (nums[nums[i]] == nums[i]) {
                return nums[i];
            }
            int temp = nums[nums[i]];
            nums[nums[i]] = nums[i];
            nums[i] = temp;
        }
        return -1;
    }
}
```

* **时间复杂度**：O(N)
* **空间复杂度**：O(1)
