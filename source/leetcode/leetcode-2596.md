---
title: 2596. 检查骑士巡视方案
date: 2023-09-13 11:43:35
tags:
  - 矩阵
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/check-knight-tour-configuration/description/

<!-- more -->

> 骑士在一张 `n x n` 的棋盘上巡视。在有效的巡视方案中，骑士会从棋盘的 **左上角** 出发，并且访问棋盘上的每个格子 **恰好一次** 。
>
> 给你一个 `n x n` 的整数矩阵 `grid` ，由范围 `[0, n * n - 1]` 内的不同整数组成，其中 `grid[row][col]` 表示单元格 `(row, col)` 是骑士访问的第 `grid[row][col]` 个单元格。骑士的行动是从下标 **0** 开始的。
>
> 如果 `grid` 表示了骑士的有效巡视方案，返回 `true`；否则返回 `false`。
>
> **注意**，骑士行动时可以垂直移动两个格子且水平移动一个格子，或水平移动两个格子且垂直移动一个格子。下图展示了骑士从某个格子出发可能的八种行动路线。
> ![img](https://images.orkva.com/images/2023/09/13/knight.png)
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/09/13/yetgriddrawio-5.png)
>
> ```
> 输入：grid = [[0,11,16,5,20],[17,4,19,10,15],[12,1,8,21,6],[3,18,23,14,9],[24,13,2,7,22]]
> 输出：true
> 解释：grid 如上图所示，可以证明这是一个有效的巡视方案。
> ```
>
> **示例 2：**
>
> ![img](https://images.orkva.com/images/2023/09/13/yetgriddrawio-6.png)
>
> ```
> 输入：grid = [[0,3,6],[5,8,1],[2,7,4]]
> 输出：false
> 解释：grid 如上图所示，考虑到骑士第 7 次行动后的位置，第 8 次行动是无效的。
> ```
>
>  
>
> **提示：**
>
> - `n == grid.length == grid[i].length`
> - `3 <= n <= 7`
> - `0 <= grid[row][col] < n * n`
> - `grid` 中的所有整数 **互不相同**

从左上角出发有些坑，并不是所有测试用例都是由左上角出发，而是需要对这种情况做特殊处理。

从当前位置，按照骑士移动的规则去判断下一个点是否在路径上，虽然有些难看，但是确实可以通过。

```java
class Solution {
    public boolean checkValidGrid(int[][] grid) {
        int n = grid.length;
        int viewed = 1;
        int x = 0;
        int y = 0;
        if (grid[x][y] != 0) {
            return false;
        }
        while (viewed < n * n) {
            if (isNextView(grid, x + 1, y + 2, viewed)) {
                x += 1;
                y += 2;
            } else if (isNextView(grid, x + 2, y + 1, viewed)) {
                x += 2;
                y += 1;
            } else if (isNextView(grid, x - 1, y + 2, viewed)) {
                x -= 1;
                y += 2;
            } else if (isNextView(grid, x + 2, y - 1, viewed)) {
                x += 2;
                y -= 1;
            } else if (isNextView(grid, x - 2, y + 1, viewed)) {
                x -= 2;
                y += 1;
            } else if (isNextView(grid, x + 1, y - 2, viewed)) {
                x += 1;
                y -= 2;
            } else if (isNextView(grid, x - 1, y - 2, viewed)) {
                x -= 1;
                y -= 2;
            } else if (isNextView(grid, x - 2, y - 1, viewed)) {
                x -= 2;
                y -= 1;
            } else {
                break ;
            }
            viewed ++;
        }
        return viewed == n * n;
    }

    public boolean isNextView(int[][] grid, int x, int y, int nextView) {
        if (isInside(x, grid.length) && isInside(y, grid.length)) {
            return grid[x][y] == nextView;
        }
        return false;
    }

    public boolean isInside(int x, int n) {
        return x >= 0 && x < n;
    }

}
```

当然，官方的解答方法还是比较舒服的。

```java
class Solution {
    public boolean checkValidGrid(int[][] grid) {
        if (grid[0][0] != 0) {
            return false;
        }
        int n = grid.length;
        int[][] indices = new int[n * n][2];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                indices[grid[i][j]][0] = i;
                indices[grid[i][j]][1] = j;
            }
        }
        for (int i = 1; i < n * n; i++) {
            int dx = Math.abs(indices[i][0] - indices[i - 1][0]);
            int dy = Math.abs(indices[i][1] - indices[i - 1][1]);
            if (dx * dy != 2) {
                return false;
            }
        }
        return true;
    }
}
```

