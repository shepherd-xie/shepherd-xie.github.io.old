---
title: 207. 课程表
date: 2023-09-11 15:53:41
tags:
  - 图
  - medium
  - leetcode
categories:
  - 算法
top:
---

https://leetcode.cn/problems/course-schedule/description/

<!-- more -->

> 你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。
>
> 在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi` 。
>
> - 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。
>
> 请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。
>
>  
>
> **示例 1：**
>
> ```
> 输入：numCourses = 2, prerequisites = [[1,0]]
> 输出：true
> 解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
> ```
>
> **示例 2：**
>
> ```
> 输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
> 输出：false
> 解释：总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
> ```
>
>  
>
> **提示：**
>
> - `1 <= numCourses <= 2000`
> - `0 <= prerequisites.length <= 5000`
> - `prerequisites[i].length == 2`
> - `0 <= ai, bi < numCourses`
> - `prerequisites[i]` 中的所有课程对 **互不相同**

显然，如果我们在所有的相关课程间添加一条有向边，那么所有的课程及这些边就构成了一个 **有向图** ，而这个题目则是为了判断这个有向图中是否有环。

```java
class Solution {
    List<Integer>[] edges;
    int[] visited;
    boolean valid = true;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        edges = new List[numCourses];
        for (int i = 0; i < edges.length; i++) {
            edges[i] = new ArrayList<>();
        }
        visited = new int[numCourses];
        for (int[] prerequisite : prerequisites) {
            edges[prerequisite[1]].add(prerequisite[0]);
        }
        for (int i = 0; i < edges.length && valid; i++) {
            if (visited[i] == 0) {
                dfs(i);
            }
        }
        return valid;
    }

    public void dfs(int course) {
        visited[course] = 1;
        for (Integer prerequisite : edges[course]) {
            if (visited[prerequisite] == 0) {
                dfs(prerequisite);
                if (!valid) {
                    return;
                }
            } else if (visited[prerequisite] == 1) {
                valid = false;
                return;
            }
        }
        visited[course] = 2;
    }
}
```

