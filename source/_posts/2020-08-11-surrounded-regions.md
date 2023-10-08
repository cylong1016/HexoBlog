---
title: 被围绕的区域
date: 2020-08-11 00:27:08
updated: 2020-08-11 00:27:08
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 深度优先搜索
    - 广度优先搜索
    - 递归
    - 回溯算法
    - 矩阵
---
---

# 题目描述

给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

**示例:**
```
输入：

X X X X
X O O X
X X O X
X O X X

运行你的函数后，矩阵变为：

X X X X
X X X X
X X X X
X O X X

```
**解释:** 被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

<!-- more -->

# 深度优先搜索

遇到矩阵的问题，无非就是广度优先搜索或者深度优先搜索，我个人比较喜欢使用递归方式的深度优先搜索，也是比较容易理解的一种方式。本题要求将所有被字母 'X' 包围的字母 'O' 都变为字母 'X' ，但很难判断哪些 'O' 是被包围的，哪些 'O' 不是被包围的。但是我们注意题目中的一句话：任何边界上的 'O' 都不会被填充为 'X'。与边界上的 'O' 相连的 'O' 也都不会被填充为 'X'。根据这个思路，我们只要遍历矩阵的边界上的 'O'，以边界上的所有 'O' 此为起点，找到所有与边界相连的字母 'O'，最后我们遍历整个矩阵，针对每个字母，如果这个字母被标记了，那么就保持原来的 'O'，如果没有被标记，那么就置为 'X' 即可。

```java
public void solve(char[][] board) {
    if (board == null || board.length == 0 || board[0].length == 0) {
        return;
    }
    int rowLen = board.length;
    int colLen = board[0].length;
    int[][] direct = new int[][]{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int directIndex = 0;
    int row = 0;
    int col = 0;
    boolean[][] visited = new boolean[rowLen][colLen];
    // 顺时针遍历矩阵的边界
    for (int i = 0; i <= ((rowLen + colLen) << 1) - 4; i++) {
        updateBoard(board, row, col, visited);
        int nextRow = row + direct[directIndex][0];
        int nextCol = col + direct[directIndex][1];
        if (nextRow < 0 || nextRow >= rowLen || nextCol < 0 || nextCol >= colLen) {
            directIndex = (directIndex + 1) % 4;
        }
        row += direct[directIndex][0];
        col += direct[directIndex][1];
    }

    // 针对每个字母，如果这个字母被标记了，那么就保持原来的 'O'，如果没有被标记，那么就置为 'X'。
    for (int i = 0; i < rowLen; i++) {
        for (int j = 0; j < colLen; j++) {
            board[i][j] = visited[i][j] ? 'O' : 'X';
        }
    }
}

public void updateBoard(char[][] board, int x, int y, boolean[][] visited) {
    if (x < 0 || y < 0 || x >= board.length || y >= board[0].length || visited[x][y] || board[x][y] == 'X') {
        return;
    }
    visited[x][y] = true;
    updateBoard(board, x + 1, y, visited);
    updateBoard(board, x, y + 1, visited);
    updateBoard(board, x - 1, y, visited);
    updateBoard(board, x, y - 1, visited);
}
```

这里使用了 `visited[x][y]` 表示是否被标记，其实可以遍历到 'O' 的时候，将此位置的字符改成任何其他字符，比如 '#'，之后遍历整个矩阵的时候，我们将其还原为 'O' 即可，这样就不用使用额外的空间来标记了。另外遍历矩阵的边界，我使用的是从 `board[0][0]` 开始顺时针遍历矩阵的边界，其实不用这么麻烦，只要分别遍历矩阵的第一行，最后一行，第一列，最后一列即可，我这样做主要是为了复习下之前的一道题：
> [顺时针打印矩阵 | 笑话人生][3]

## 复杂度分析

* 时间复杂度：O(n × m)，其中 n 和 m 分别为矩阵的行数和列数。深度优先搜索过程中，每一个点至多只会被标记一次。
* 空间复杂度：O(n × m)，其中 n 和 m 分别为矩阵的行数和列数。主要为深度优先搜索的栈的开销。

# 来源

> [被围绕的区域 | 力扣（LeetCode）][1]
> [被围绕的区域 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/surrounded-regions/ "被围绕的区域 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/surrounded-regions/solution/bei-wei-rao-de-qu-yu-by-leetcode-solution/ "被围绕的区域 | 题解（LeetCode）"
[3]: /blog/2020/06/05/shun-shi-zhen-da-yin-ju-zhen-lcof/ "顺时针打印矩阵 | 笑话人生"
