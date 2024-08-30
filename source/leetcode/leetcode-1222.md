---
title: 1222. 可以攻击国王的皇后
date: 2023-09-14 15:34:30
tags:
  - 矩阵
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/queens-that-can-attack-the-king/description/

<!-- more -->

> 在一个 **8x8** 的棋盘上，放置着若干「黑皇后」和一个「白国王」。
>
> 给定一个由整数坐标组成的数组 `queens` ，表示黑皇后的位置；以及一对坐标 `king` ，表示白国王的位置，返回所有可以攻击国王的皇后的坐标(任意顺序)。
>
>  
>
> **示例 1：**
>
> ![img](https://images.orkva.com/images/2023/09/14/untitled-diagram.jpg)
>
> ```
> 输入：queens = [[0,1],[1,0],[4,0],[0,4],[3,3],[2,4]], king = [0,0]
> 输出：[[0,1],[1,0],[3,3]]
> 解释： 
> [0,1] 的皇后可以攻击到国王，因为他们在同一行上。 
> [1,0] 的皇后可以攻击到国王，因为他们在同一列上。 
> [3,3] 的皇后可以攻击到国王，因为他们在同一条对角线上。 
> [0,4] 的皇后无法攻击到国王，因为她被位于 [0,1] 的皇后挡住了。 
> [4,0] 的皇后无法攻击到国王，因为她被位于 [1,0] 的皇后挡住了。 
> [2,4] 的皇后无法攻击到国王，因为她和国王不在同一行/列/对角线上。
> ```
>
> **示例 2：**
>
> **![img](https://images.orkva.com/images/2023/09/14/untitled-diagram-1.jpg)**
>
> ```
> 输入：queens = [[0,0],[1,1],[2,2],[3,4],[3,5],[4,4],[4,5]], king = [3,3]
> 输出：[[2,2],[3,4],[4,4]]
> ```
>
> **示例 3：**
>
> **![img](https://images.orkva.com/images/2023/09/14/untitled-diagram-2.jpg)**
>
> ```
> 输入：queens = [[5,6],[7,7],[2,1],[0,7],[1,6],[5,1],[3,7],[0,3],[4,0],[1,2],[6,3],[5,0],[0,4],[2,2],[1,1],[6,4],[5,4],[0,0],[2,6],[4,5],[5,2],[1,4],[7,5],[2,3],[0,5],[4,2],[1,0],[2,7],[0,1],[4,6],[6,1],[0,6],[4,3],[1,7]], king = [3,4]
> 输出：[[2,3],[1,4],[1,6],[3,7],[4,3],[5,4],[4,5]]
> ```
>
>  
>
> **提示：**
>
> - `1 <= queens.length <= 63`
> - `queens[i].length == 2`
> - `0 <= queens[i][j] < 8`
> - `king.length == 2`
> - `0 <= king[0], king[1] < 8`
> - 一个棋盘格上最多只能放置一枚棋子。

### 方法一：遍历王后，查找可以攻击到国王的位置

王后可以攻击到国王的情况：

1. 国王在王后的攻击范围内，即横、竖、交叉线四线中
2. 国王与王后之间没有其他棋子

所以，要解决这个问题，首先需要判断国王是否在王后的攻击范围

* 在横竖直线上表示两点横坐标或纵坐标相同，即 `queen[x] - king[x] == 0` 或 `queen[y] - king[y] == y` ，简化则是 `(queen[x] - king[x]) * (queen[y] - king[y]) == 0`
* 在交叉斜线上则有 `|queen[x] - king[x]| == |queen[y] - king[y]|`

当判断出可以攻击之后，下一步就要判断国王与王后之前是否有其他棋子

1. 我们首先定义一个二维数组表示棋盘，遍历王后将王后置于棋盘上
2. 由于两点可确定一条直线，即由国王与王后间构成的一条直线
3. 从两点中横坐标较小的一侧出发，通过直线方程计算出纵坐标，再通过棋盘查找该点是否有棋子

```java
class Solution {
    public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
        boolean[][] grid = new boolean[8][8];
        for (int[] queen : queens) {
            grid[queen[0]][queen[1]] = true;
        }
        List<List<Integer>> res = new ArrayList<>();
        for (int[] queen : queens) {
            if (canAttack(queen, king)) {
                boolean noInterval = true;
                if (queen[0] - king[0] == 0) {
                    for (int i = Math.min(queen[1], king[1]) + 1; i < Math.max(queen[1], king[1]); i ++) {
                        if (grid[king[0]][i]) {
                            noInterval = false;
                            break;
                        }
                    }
                } else if (queen[1] - king[1] == 0) {
                    for (int i = Math.min(queen[0], king[0]) + 1; i < Math.max(queen[0], king[0]); i ++) {
                        if (grid[i][king[1]]) {
                            noInterval = false;
                            break;
                        }
                    }
                } else {
                    for (int i = Math.min(queen[0], king[0]) + 1; i < Math.max(queen[0], king[0]); i ++) {
                        if (grid[i][lineFun(i, queen, king)]) {
                            noInterval = false;
                            break;
                        }
                    }
                }
                if (noInterval) {
                    List<Integer> posList = new ArrayList<Integer>();
                    posList.add(queen[0]);
                    posList.add(queen[1]);
                    res.add(posList);
                }
            }
        }
        return res;
    }

    public boolean canAttack(int[] queen, int[] king) {        
        int dx = queen[0] - king[0];
        int dy = queen[1] - king[1];
        return dx * dy == 0 || Math.abs(dx) - Math.abs(dy) == 0;
    }

    public int lineFun(int x, int[] point1, int[] point2) {
        double dx1 = x - point1[0];
        double dx2 = point2[0] - point1[0];
        double dy = point2[1] - point1[1];
        return (int) (dx1 / dx2 * dy + point1[1]);
    }
}
```

### 方法二：统计能被国王「看到」的皇后（by [灵茶山艾府](https://leetcode.cn/problems/queens-that-can-attack-the-king/solutions/2441443/tong-ji-neng-bei-guo-wang-kan-dao-de-hua-08s6/)）

能攻击到国王的皇后，需要满足：

皇后与国王在同一行，或者同一列，或者同一斜线。
皇后与国王之间没有棋子。换句话说，皇后不能被其它皇后挡住。
一种思路是枚举每个皇后，去判断是否满足上述条件。

更加巧妙的做法是，站在国王的视角，计算有哪些皇后能被国王「看到」。

想象成从国王的位置发射八个方向的射线，记录每条射线首次遇到的皇后。

```java
class Solution {
    private final static int[][] directions = {{1, 0}, {1, 1}, {0, 1}, {-1, 1}, {-1, 0}, {-1, -1}, {0, -1}, {1, -1}};

    public List<List<Integer>> queensAttacktheKing(int[][] queens, int[] king) {
        boolean[][] isQueen = new boolean[8][8]; // 数组效率比哈希表高
        for (int[] q : queens) {
            isQueen[q[0]][q[1]] = true;
        }
        List<List<Integer>> ans = new ArrayList<>();
        for (int[] d : directions) {
            int x = king[0] + d[0];
            int y = king[1] + d[1];
            while (0 <= x && x < 8 && 0 <= y && y < 8) {
                if (isQueen[x][y]) {
                    ans.add(List.of(x, y));
                    break;
                }
                x += d[0];
                y += d[1];
            }
        }
        return ans;
    }
}
```
