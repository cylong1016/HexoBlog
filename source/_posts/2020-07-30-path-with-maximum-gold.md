---
title: 黄金矿工
date: 2020-07-30 23:45:14
updated: 2020-07-30 23:45:14
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 数组
    - 递归
    - 深度优先搜索
    - 回溯算法
---
---

# 题目描述

你要开发一座金矿，地质勘测学家已经探明了这座金矿中的资源分布，并用大小为 m * n 的网格 grid 进行了标注。每个单元格中的整数就表示这一单元格中的黄金数量；如果该单元格是空的，那么就是 0。为了使收益最大化，矿工需要按以下规则来开采黄金：

* 每当矿工进入一个单元，就会收集该单元格中的所有黄金。
* 矿工每次可以从当前位置向上下左右四个方向走。
* 每个单元格只能被开采（进入）一次。
* 不得开采（进入）黄金数目为 0 的单元格。
* 矿工可以从网格中任意一个有黄金的单元格出发或者是停止。

**示例 1：**
```
输入：grid = [[0, 6, 0], [5, 8, 7], [0, 9, 0]]
输出：24
解释：
[[0, 6, 0],
 [5, 8, 7],
 [0, 9, 0]]
一种收集最多黄金的路线是：9 -> 8 -> 7。
```

**示例 2：**
```
输入：grid = [[1, 0, 7], [2, 0, 6], [3, 4, 5], [0, 3, 0], [9, 0, 20]]
输出：28
解释：
[[1, 0, 7],
 [2, 0, 6],
 [3, 4, 5],
 [0, 3, 0],
 [9, 0, 20]]
一种收集最多黄金的路线是：1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7。
```

**提示：**

* 1 <= grid.length, grid[i].length <= 15
* 0 <= grid[i][j] <= 100
* 最多 25 个单元格中有黄金。

<!-- more -->

# 题解

首先矿工可以从网格中任意一个有黄金的单元格出发或者是停止。于是我们将循环遍历网格全部的有黄金的点，作为起点。接下来，我们进行递归处理，每次递归的时候记录 gold 的值，传到下一次递归中，同时有一个全局变量 max 记录最大的黄金数，每次递归的总黄金数就是 `gold + grid[i][j]`。递归终止的时候，我们用 gold 值来更新 max 值。现在的问题是，我们如何处理递归的终止条件。这里，我们同时使用回溯算法，定义 visit 的 boolean 型二维数组，每次递归的时候，我们将当前节点置为 true 表示当前节点已经访问，递归回溯的时候，我们置为 false，表示当前节点没有被访问。于是我们得到递归的终止条件如下：

* i < 0 || i >= grid.length
* j < 0 || j >= grid[0].length
* grid[i][j] == 0
* visit[i][j] == true

所有节点为起点，进行深度优先搜索，最终的 max 值就是最大黄金数。

```java
int max = 0;

public int getMaximumGold(int[][] grid) {
    boolean[][] visit = new boolean[grid.length][grid[0].length];
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] > 0) {
                dfsGetMaximumGold(grid, i, j, 0, visit);
            }
        }
    }
    return max;
}

public void dfsGetMaximumGold(int[][] grid, int i, int j, int gold, boolean[][] visit) {
    if ((i < 0 || i >= grid.length) || (j < 0 || j >= grid[0].length) || grid[i][j] == 0 || visit[i][j]) {
        max = Math.max(max, gold);
        return;
    }
    gold += grid[i][j];
    visit[i][j] = true;
    dfsGetMaximumGold(grid, i + 1, j, gold, visit);
    dfsGetMaximumGold(grid, i - 1, j, gold, visit);
    dfsGetMaximumGold(grid, i, j + 1, gold, visit);
    dfsGetMaximumGold(grid, i, j - 1, gold, visit);
    visit[i][j] = false;
}
```

# 来源
> [黄金矿工 | 力扣（LeetCode）][1]
> [黄金矿工 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/path-with-maximum-gold/ "黄金矿工 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/path-with-maximum-gold/solution/ "黄金矿工 | 题解（LeetCode）"
