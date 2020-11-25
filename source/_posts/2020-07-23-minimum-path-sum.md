---
title: 最小路径和
date: 2020-07-23 09:24:20
updated: 2020-07-23 09:24:20
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 动态规划
    - 递归
    - 记忆化
    - 深度优先搜索
    - 数组
---
---

# 题目描述

给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：** 每次只能向下或者向右移动一步。

**示例:**
```
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。
```

<!-- more -->

# 动态规划

此题是典型的动态规划问题，由于路径的方向只能是向下或向右，因此网格的第一行的每个元素只能从左上角元素开始向右移动到达，网格的第一列的每个元素只能从左上角元素开始向下移动到达，此时的路径是唯一的，因此每个元素对应的最小路径和即为对应的路径上的数字总和。对于不在第一行和第一列的元素，可以从其上方相邻元素向下移动一步到达，或者从其左方相邻元素向右移动一步到达，元素对应的最小路径和等于其上方相邻元素与其左方相邻元素两者对应的最小路径和中的最小值加上当前元素的值。

创建二维数组 dp，与原始网格的大小相同，dp[i][j] 表示从左上角出发到 (i, j) 位置的最小路径和。显然，dp[0][0]=grid[0][0]。对于 dp 中的其余元素，通过以下状态转移方程计算元素值。

* 当 i > 0 且 j = 0 时，dp[i][0] = dp[i − 1][0] + grid[i][0]。
* 当 i = 0 且 j > 0 时，dp[0][j] = dp[0][j − 1] + grid[0][j]。
* 当 i > 0 且 j > 0 时，dp[i][j] = min(dp[i − 1][j], dp[i][j − 1]) + grid[i][j]。

最后得到 dp[m − 1][n − 1] 的值即为从网格左上角到网格右下角的最小路径和。

```java
public int minPathSum(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }
    int row = grid.length;
    int col = grid[0].length;
    int[][] dp = new int[row][col];
    dp[0][0] = grid[0][0];
    for (int i = 1; i < row; i++) {
        dp[i][0] = grid[i][0] + dp[i - 1][0];
    }
    for (int j = 1; j < col; j++) {
        dp[0][j] = grid[0][j] + dp[0][j - 1];
    }
    for (int i = 1; i < row; i++) {
        dp[i][0] = dp[i - 1][0] + grid[i][0];
        for (int j = 1; j < col; j++) {
            dp[i][j] = grid[i][j] + Math.min(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    return dp[row - 1][col - 1];
}
```

## 复杂度分析

* 时间复杂度：Ο(mn)，其中 m 和 n 分别是网格的行数和列数。需要对整个网格遍历一次，计算 dp 的每个元素的值。
* 空间复杂度：O(mn)，其中 m 和 n 分别是网格的行数和列数。创建一个二维数组 dp，和网格大小相同。空间复杂度可以优化，例如每次只存储上一行的 dp 值，则可以将空间复杂度优化到 O(n)。

# 递归

在我不会动态规划之前，其实第一次想到的会是递归处理，不过现在会了，还是动态规划香哈哈哈，在这里我仅仅说一下递归解题思路。

我们从 [0, 0] 坐标出发，最短路径是 `f[0][0] + min(f[0][1], f[1][0])` ，针对 f[0]\[1\] 和 f[1][0] 继续进行递归处理。这里需要注意的一点是，我们在计算 f[0][1] 和 f[1][0] 的时候，会重复计算 f[0]\[1\]，所以我们使用一个 min[i][j] 数组保存 [i, j] 坐标到右下角的最小路径，这样就避免的重复计算。

# 来源

> [最小路径和 | 力扣（LeetCode）][1]
> [最小路径和 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/minimum-path-sum/ "最小路径和 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/minimum-path-sum/solution/zui-xiao-lu-jing-he-by-leetcode-solution/ "最小路径和 | 题解（LeetCode）"
