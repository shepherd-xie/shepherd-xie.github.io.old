---
title: 1267. 统计参与通信的服务器
date: 2023-08-03 15:04:50
tags:
  - 矩阵
  - medium
  - leetcode
categories:
  - 算法
top:
---

以为写的够难看的，结果还给过了。。。

https://leetcode.cn/problems/count-servers-that-communicate/description/

<!-- more -->

> 这里有一幅服务器分布图，服务器的位置标识在 `m * n` 的整数矩阵网格 `grid` 中，1 表示单元格上有服务器，0 表示没有。
>
> 如果两台服务器位于同一行或者同一列，我们就认为它们之间可以进行通信。
>
> 请你统计并返回能够与至少一台其他服务器进行通信的服务器的数量。
> 
> 
> 
>**示例 1：**
> 
> ![img](https://images.orkva.com/images/2023/08/24/untitled-diagram-6.jpg)
>
> ```
>输入：grid = [[1,0],[0,1]]
> 输出：0
>解释：没有一台服务器能与其他服务器进行通信。
> ```
>
> **示例 2：**
>
> **![img](https://images.orkva.com/images/2023/08/24/untitled-diagram-4-1.jpg)**
>
> ```
>输入：grid = [[1,0],[1,1]]
> 输出：3
>解释：所有这些服务器都至少可以与一台别的服务器进行通信。
>  ```
>
> **示例 3：**
>
> ![img](https://images.orkva.com/images/2023/08/24/untitled-diagram-1-3.jpg)
> 
> ```
> 输入：grid = [[1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1]]
> 输出：4
> 解释：第一行的两台服务器互相通信，第三列的两台服务器互相通信，但右下角的服务器无法与其他服务器通信。
> ```
> 
>  
> 
> **提示：**
> 
> - `m == grid.length`
> - `n == grid[i].length`
> - `1 <= m <= 250`
> - `1 <= n <= 250`
> - `grid[i][j] == 0 or 1`

关键点在于如何去判断当前节点可以与其他节点连接，以及当前节点是否已经计算过。

我们在遍历的过程中在原始矩阵做修改，定义 2 表示当前节点已经计算过。

1. 遍历矩阵，遇到服务器为 1 时进行判断
   1. 当前服务器是未被计算的情况下，寻找是否可以连接其他服务器
   2. 遍历当前节点所在的行列，如果有其他服务器，则 `res += 1` 并将其置为 2 表示已经计算过，同时定义 `link` 变量表示当前服务器可以与其他服务器通信。
   3. 行列遍历过后查看 `link` ，如果可以和其他节点通信，则设当前节点为 2 ，并且 `res += 1` 。
2. 遍历结束，返回 `res` 。

```java
class Solution {
    public int countServers(int[][] grid) {
        int res = 0;
        int m = grid.length;
        int n = grid[0].length;
        for (int i = 0; i < m; i ++) {
            for (int j = 0; j < n; j ++) {
                // 当前节点存在服务器
                if (grid[i][j] == 1) {
                    int link = 0; // 不清楚当前服务器能否与其他服务器通信
                    // 查找当前列是否有其他服务器
                    for (int k = 0; k < m; k ++) {
                        // 有服务器且不是当前节点
                        if (grid[k][j] != 0 && k != i) {
                            // 标记为可以通信
                            link = 1;
                            // 如果该服务器没有计算过
                            if (grid[k][j] == 1) {
                                // 结果值 +1
                                res ++;
                                // 标记为已计算
                                grid[k][j] = 2;
                            }
                        }
                    }
                    // 查找当前行是否有其他服务器
                    for (int k = 0; k < n; k ++) {
                        if (grid[i][k] != 0 && k != j) {
                            link = 1;
                            if (grid[i][k] == 1) {
                                res ++;
                                grid[i][k] = 2;
                            }
                        }
                    }
                    // 如果可以通信
                    if (link == 1) {
                        res ++;
                        grid[i][j] = 2;
                    }
                }
            }
        }
        return res;
    }
}
```

当然我们可以添加额外的数组以保证原始数据不被修改。
