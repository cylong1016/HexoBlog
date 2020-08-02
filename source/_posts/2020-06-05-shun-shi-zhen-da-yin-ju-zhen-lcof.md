---
title: 顺时针打印矩阵
date: 2020-06-05 02:05:55
updated: 2020-06-05 02:05:55
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 数组
    - 回溯算法
    - 指针
    - 记忆化
---
---

# 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

**示例 1：**
> 输入：matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
> 输出：[1, 2, 3, 6, 9, 8, 7, 4, 5]

**示例 2：**
> 输入：matrix = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
> 输出：[1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]

**限制：**
> 0 <= matrix.length <= 100
> 0 <= matrix[i].length <= 100

<!-- more -->

# 题解

可以模拟打印矩阵的路径。初始位置是矩阵的左上角，初始方向是向右，当路径超出界限或者进入之前访问过的位置时，则顺时针旋转，进入下一个方向。判断路径是否进入之前访问过的位置需要使用一个与输入矩阵大小相同的辅助矩阵 visited，其中的每个元素表示该位置是否被访问过。当一个元素被访问时，将 visited 中的对应位置的元素设为已访问。

如何判断路径是否结束？由于矩阵中的每个元素都被访问一次，因此路径的长度即为矩阵中的元素数量，当路径的长度达到矩阵中的元素数量时即为完整路径，将该路径返回。

```java
public int[] spiralOrder(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return new int[0];
    }
    int rows = matrix.length;
    int columns = matrix[0].length;
    // 判断路径是否被访问过
    boolean[][] visited = new boolean[rows][columns];
    int total = rows * columns;
    // 总路径长度为矩阵的元素数量
    int[] order = new int[total];
    int row = 0;
    int column = 0;
    // 移动的方向
    int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int directionIndex = 0;
    for (int i = 0; i < total; i++) {
        order[i] = matrix[row][column];
        visited[row][column] = true;
        int nextRow = row + directions[directionIndex][0];
        int nextColumn = column + directions[directionIndex][1];
        if (nextRow < 0 || nextRow >= rows || nextColumn < 0 || nextColumn >= columns
                || visited[nextRow][nextColumn]) {
            directionIndex = (directionIndex + 1) % 4;
        }
        row += directions[directionIndex][0];
        column += directions[directionIndex][1];
    }
    return order;
}
```

# 复杂度分析

* 时间复杂度：O(mn)，其中 m 和 n 分别是输入矩阵的行数和列数。矩阵中的每个元素都要被访问一次。
* 空间复杂度：O(mn)。需要创建一个大小为 m × n 的矩阵 visited 记录每个位置是否被访问过。

# 来源
> [顺时针打印矩阵 | 力扣（LeetCode）][1]
> [顺时针打印矩阵 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/ "顺时针打印矩阵 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/solution/shun-shi-zhen-da-yin-ju-zhen-by-leetcode-solution/ "顺时针打印矩阵 | 题解（LeetCode）"
