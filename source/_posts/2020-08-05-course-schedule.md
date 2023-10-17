---
title: 课程表
date: 2020-08-05 02:43:14
updated: 2020-08-05 02:43:14
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 深度优先搜索
    - 广度优先搜索
    - 图
    - 拓扑排序
    - 递归
    - 回溯算法
    - 数组
---
---

# 题目描述

你这个学期必须选修 numCourse 门课程，记为 0 到 numCourse - 1 。在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：[0, 1]。给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

**示例 1:**
> 输入: 2, [[1, 0]] 
> 输出: true
> 解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。

**示例 2:**
> 输入: 2, [[1, 0], [0, 1]]
> 输出: false
> 解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。

<!-- more -->

# 题解

本题是一道经典的「拓扑排序」问题。给定一个包含 n 个节点的有向图 G，我们给出它的节点编号的一种排列，如果满足：
> 对于图 G 中的任意一条有向边 (u, v)，u 在排列中都出现在 v 的前面。那么称该排列是图 G 的「拓扑排序」。

根据上述的定义，我们可以得出两个结论：
* 如果图 G 中存在环（即图 G 不是「有向无环图」），那么图 G 不存在拓扑排序。这是因为假设图中存在环 x₁，x₂，x₃，……xn，x₁，那么 x₁ 在排列中必须出现在 xn 的前面，但是 xn 同时也出现在 x₁ 前面，因此不存在一个满足要求的排列，也就不存在拓扑排序。
* 如果图 G 是有向无环图，那么它的拓扑排序可能不止一种。举一个最极端的例子，如果图 G 值包含 n 个节点却没有任何边，那么任意一种编号的排列都可以作为拓扑排序。

有了上述的简单分析，我们就可以将本题建模成一个求拓扑排序的问题了：
* 我们将每一门课程看成一个节点；
* 如果想要学习课程 A 之前必须完成课程 B，那么我们从 B 到 A 连接一条有向边。这样以来，在拓扑排序中，B 一定出现在 A 的前面。

# 深度优先搜索

对于图中的任意一个节点，它在搜索的过程中有三种状态，即：
* 「未搜索」：我们还没有搜索到这个节点；
* 「搜索中」：我们搜索过这个节点，但还没有回溯到该节点，即该节点还没有入栈，还有相邻的节点没有搜索完成）；
* 「已完成」：我们搜索过并且回溯过这个节点，即该节点已经入栈，并且所有该节点的相邻节点都出现在栈的更底部的位置，满足拓扑排序的要求。

通过上述的三种状态，我们就可以给出使用深度优先搜索得到拓扑排序的算法流程，在每一轮的搜索搜索开始时，我们任取一个「未搜索」的节点开始进行深度优先搜索。
* 我们将当前搜索的节点 u 标记为「搜索中」，遍历该节点的每一个相邻节点 v：
* 如果 v 为「未搜索」，那么我们开始搜索 v，待搜索完成回溯到 u；
* 如果 v 为「搜索中」，那么我们就找到了图中的一个环，因此是不存在拓扑排序的；
* 如果 v 为「已完成」，那么说明 v 已经在栈中了，而 u 还不在栈中，因此 u 无论何时入栈都不会影响到 (u, v) 之前的拓扑关系，以及不用进行任何操作。
* 当 u 的所有相邻节点都为「已完成」时，我们将 u 放入栈中，并将其标记为「已完成」。

在整个深度优先搜索的过程结束后，如果我们没有找到图中的环，那么栈中存储这所有的 n 个节点，从栈顶到栈底的顺序即为一种拓扑排序。

```java
List<List<Integer>> edges;
int[] visited;
boolean valid = true;

public boolean canFinish(int numCourses, int[][] prerequisites) {
    edges = new ArrayList<>();
    for (int i = 0; i < numCourses; ++i) {
        edges.add(new ArrayList<>());
    }
    visited = new int[numCourses];
    for (int[] info : prerequisites) {
        edges.get(info[1]).add(info[0]);
    }
    for (int i = 0; i < numCourses && valid; i++) {
        if (visited[i] == 0) {
            dfsCourse(i);
        }
    }
    return valid;
}

public void dfsCourse(int u) {
    visited[u] = 1;
    for (int v : edges.get(u)) {
        if (visited[v] == 0) {
            dfsCourse(v);
            if (!valid) {
                return;
            }
        } else if (visited[v] == 1) {
            valid = false;
            return;
        }
    }
    visited[u] = 2;
}
```

## 复杂度分析

* 时间复杂度: O(n + m)，其中 n 为课程数，m 为先修课程的要求数。这其实就是对图进行深度优先搜索的时间复杂度。
* 空间复杂度: O(n + m)。题目中是以列表形式给出的先修课程关系，为了对图进行深度优先搜索，我们需要存储成邻接表的形式，空间复杂度为 O(n + m)。在深度优先搜索的过程中，我们需要最多 O(n) 的栈空间（递归）进行深度优先搜索，因此总空间复杂度为 O(n + m)。

# 广度优先搜索

拓扑排序也可以使用广度优先搜索实现。我们考虑拓扑排序中最前面的节点，该节点一定不会有任何入边，也就是它没有任何的先修课程要求。当我们将一个节点加入答案中后，我们就可以移除它的所有出边，代表着它的相邻节点少了一门先修课程的要求。如果某个相邻节点变成了「没有任何入边的节点」，那么就代表着这门课可以开始学习了。按照这样的流程，我们不断地将没有入边的节点加入答案，直到答案中包含所有的节点（得到了一种拓扑排序）或者不存在没有入边的节点（图中包含环）。

我们使用一个队列来进行广度优先搜索。初始时，所有入度为 0 的节点都被放入队列中，它们就是可以作为拓扑排序最前面的节点，并且它们之间的相对顺序是无关紧要的。在广度优先搜索的每一步中，我们取出队首的节点 u：
* 我们将 u 放入答案中；
* 我们移除 u 的所有出边，也就是将 u 的所有相邻节点的入度减少 1。如果某个相邻节点 v 的入度变为 0，那么我们就将 v 放入队列中。

在广度优先搜索的过程结束后。如果答案中包含了这 n 个节点，那么我们就找到了一种拓扑排序，否则说明图中存在环，也就不存在拓扑排序了。

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    List<List<Integer>> edges = new ArrayList<>();
    int[] degree = new int[numCourses];
    for (int i = 0; i < numCourses; i++) {
        edges.add(new ArrayList<>());
    }

    for (int[] courseRelation : prerequisites) {
        edges.get(courseRelation[1]).add(courseRelation[0]);
        degree[courseRelation[0]]++;
    }

    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < degree.length; i++) {
        if (degree[i] == 0) {
            queue.offer(i);
        }
    }

    int visited = 0;

    while (!queue.isEmpty()) {
        visited++;
        int beforeCourse = queue.poll();
        for (int afterCourse : edges.get(beforeCourse)) {
            if (--degree[afterCourse] == 0) {
                queue.offer(afterCourse);
            }
        }
    }
    return visited == numCourses;
}
```

## 复杂度分析

* 时间复杂度: O(n + m)，其中 n 为课程数，m 为先修课程的要求数。这其实就是对图进行广度优先搜索的时间复杂度。
* 空间复杂度: O(n + m)。题目中是以列表形式给出的先修课程关系，为了对图进行广度优先搜索，我们需要存储成邻接表的形式，空间复杂度为 O(n + m)。在广度优先搜索的过程中，我们需要最多 O(n) 的队列空间（迭代）进行广度优先搜索。因此总空间复杂度为 O(n + m)。

# 来源

> [课程表 | 力扣（LeetCode）][1]
> [课程表 | 题解（LeetCode）][2]
                  
---

[1]: https://leetcode-cn.com/problems/course-schedule/ "课程表 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/course-schedule/solution/ke-cheng-biao-by-leetcode-solution/ "课程表 | 题解（LeetCode）"
