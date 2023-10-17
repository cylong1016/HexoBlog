---
title: 扫雷游戏
date: 2020-07-28 19:48:22
updated: 2020-07-28 19:48:22
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 数组
    - 递归
    - 深度优先搜索
    - 矩阵
---
---

# 題目描述

让我们一起来玩扫雷游戏！

给定一个代表游戏板的二维字符矩阵。 'M' 代表一个未挖出的地雷，'E' 代表一个未挖出的空方块，'B' 代表没有相邻（上，下，左，右，和所有4个对角线）地雷的已挖出的空白方块，数字（'1' 到 '8'）表示有多少地雷与这块已挖出的方块相邻，'X' 则表示一个已挖出的地雷。现在给出在所有未挖出的方块中（'M'或者'E'）的下一个点击位置（行和列索引），根据以下规则，返回相应位置被点击后对应的面板：

1. 如果一个地雷（'M'）被挖出，游戏就结束了- 把它改为 'X'。
2. 如果一个没有相邻地雷的空方块（'E'）被挖出，修改它为（'B'），并且所有和其相邻的未挖出方块都应该被递归地揭露。
3. 如果一个至少与一个地雷相邻的空方块（'E'）被挖出，修改它为数字（'1'到'8'），表示相邻地雷的数量。
4. 如果在此次点击中，若无更多方块可被揭露，则返回面板。

**示例 1：**
```
输入:

[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]

Click : [3,0]

输出:

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

**解释:**
{% asset_img minesweeper_example_1.png 示例1 %}

**示例 2：**
```
输入:

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Click : [1,2]

输出:

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

**解释:**
{% asset_img minesweeper_example_2.png 示例2 %}

**注意：**

1. 输入矩阵的宽和高的范围为 [1, 50]。
2. 点击的位置只能是未被挖出的方块 ('M' 或者 'E')，这也意味着面板至少包含一个可点击的方块。
3. 输入面板不会是游戏结束的状态（即有地雷已被挖出）。
4. 简单起见，未提及的规则在这个问题中可被忽略。例如，当游戏结束时你不需要挖出所有地雷，考虑所有你可能赢得游戏或标记方块的情况。

<!-- more -->

# 题解

根据题意，我们点击某个方块后，如果是地雷 M，则直接修改为 X 返回。否则我们计算以当前点击的方块为中心的九宫格内地雷的数量，如果没有地雷，则我们将当前节点标记为 B 并递归处理当前节点九宫格内的其他节点。否则我们将当前节点标记为周围地雷的数量，并结束递归。

```java
public char[][] updateBoard(char[][] board, int[] click) {
    if (board[click[0]][click[1]] == 'M') {
        board[click[0]][click[1]] = 'X';
        return board;
    } else {
        checkBoard(board, click[0], click[1]);
    }
    return board;
}

private void checkBoard(char[][] board, int x, int y) {
    int row = board.length;
    int col = board[0].length;
    if (board[x][y] != 'M' && board[x][y] != 'E') {
        return;
    }
    board[x][y] = 0;
    // 计算点击节点的九宫格内地雷数量
    for (int i = x - 1; i <= x + 1; i++) {
        for (int j = y - 1; j <= y + 1; j++) {
            if (i >= 0 && j >= 0 && i < row && j < col) {
                board[x][y] += (board[i][j] == 'M' ? 1 : 0);
            }
        }
    }
    // 如果点击节点九宫格内没有地雷，则递归处理九宫格内其他的节点
    if (board[x][y] == 0) {
        board[x][y] = 'B';
        for (int i = x - 1; i <= x + 1; i++) {
            for (int j = y - 1; j <= y + 1; j++) {
                if (i >= 0 && j >= 0 && i < row && j < col) {
                    checkBoard(board, i, j);
                }
            }
        }
    } else {
        board[x][y] += '0';
    }
}
```

# 来源

> [扫雷游戏 | 力扣（LeetCode）][1]
> [扫雷游戏 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/minesweeper/ "扫雷游戏 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/minesweeper/solution/ "扫雷游戏 | 题解（LeetCode）"
