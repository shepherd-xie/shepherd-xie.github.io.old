---
title: 169. 多数元素
date: 2023-09-27 11:53:59
tags:
  - 数组
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/majority-element/description/

<!-- more -->

>给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。
>
>你可以假设数组是非空的，并且给定的数组总是存在多数元素。
>
> 
>
> **示例 1：**
>
>```
>输入：nums = [3,2,3]
>输出：3
>```
>
>**示例 2：**
>
>```
>输入：nums = [2,2,1,1,1,2,2]
>输出：2
>```
>
> 
>
>**提示：**
>
>- `n == nums.length`
>- `1 <= n <= 5 * 104`
>- `-109 <= nums[i] <= 109`
>
> 
>
>**进阶：**尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

#### 哈希表

```java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> hash = new HashMap<Integer, Integer>();
        for (int n : nums) {
            hash.put(n, hash.getOrDefault(n, 0) + 1);
        }
        return hash.entrySet().stream()
                .max(Comparator.comparingInt(Map.Entry::getValue))
                .map(Map.Entry::getKey).get();
    }
}
```

### 排序

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }
}
```

### Boyer-Moore 投票算法

```java
class Solution {
    public int majorityElement(int[] nums) {
        int candidate = 0;
        int count = 0;
        for (int n : nums) {
            if (count == 0) {
                candidate = n;
            }
            if (candidate == n) {
                count ++;
            } else {
                count --;
            }
        }
        return candidate;
    }
}
```

