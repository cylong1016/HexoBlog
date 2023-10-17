---
title: Z 字形变换
date: 2019-12-30 15:03:27
updated: 2019-12-30 15:03:27
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 字符串
    - 指针
---
---

# 题目描述

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
```
L   C   I   R
E T O E S I I G
E   D   H   N
```
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

**示例 1:**
> 输入: s = "LEETCODEISHIRING", numRows = 3
> 输出: "LCIRETOESIIGEDHN"

**示例 2:**
> 输入: s = "LEETCODEISHIRING", numRows = 4
> 输出: "LDREOEIIECIHNTSG"

解释:
```
L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

<!-- more -->

# 题解

首先我们可以知道numRows为行数，输出的时候我们只要把每行的字符按顺序输出即可，其实上面的输出，为了美观，使用了空格，当我们把空格拿掉，就转换成了下面的输出。
```
L C I R
E T O E S I I G
E D H N
```
那么我们只要知道第i个字符，要放到哪一行即可。

```java
public String convert(String s, int numRows) {
    if (numRows <= 1) {
        return s;
    }
    List<List<Character>> zList = new ArrayList<>(numRows);
    for (int i = 0; i < numRows; i++) {
        zList.add(new ArrayList<>());
    }
    int j; // 判断字符放到哪一行
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        // 多少字母为一个循环
        int cycle = numRows + numRows - 2;
        j = i % cycle;
        j = Math.min(cycle - j, j);
        zList.get(j).add(c);
    }
    StringBuilder sBuilder = new StringBuilder();
    for (int i = 0; i < numRows; i++) {
        zList.get(i).forEach(sBuilder::append);
    }
    return sBuilder.toString();
}
```

官方解法和我的思路差不多，只不过他细节上处理的比我好，行数是 `numRows` 和 `len(s)` 的较小值，其每行数据没有使用`List<Character>`，而是使用StringBuilder，另外使用了方向变量`goingDown`来判断此时在哪一行。以下是官方解法：

我们可以使用 `min(numRows, len(s))` 个列表来表示 Z 字形图案中的非空行。从左到右迭代 s，将每个字符添加到合适的行。可以使用当前行和当前方向这两个变量对合适的行进行跟踪。只有当我们向上移动到最上面的行或向下移动到最下面的行时，当前方向才会发生改变。

```java
public String convert(String s, int numRows) {
    if (numRows == 1)
        return s;

    List<StringBuilder> rows = new ArrayList<>();
    for (int i = 0; i < Math.min(numRows, s.length()); i++)
        rows.add(new StringBuilder());

    int curRow = 0;
    boolean goingDown = false;
    for (char c : s.toCharArray()) {
        rows.get(curRow).append(c);
        if (curRow == 0 || curRow == numRows - 1)
            goingDown = !goingDown;
        curRow += goingDown ? 1 : -1;
    }

    StringBuilder ret = new StringBuilder();
    for (StringBuilder row : rows)
        ret.append(row);
    return ret.toString();
}
```

# 来源

> [Z 字形变换 | 力扣（LeetCode）][1]
> [Z 字形变换 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/zigzag-conversion/ "Z 字形变换 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/zigzag-conversion/solution/z-zi-xing-bian-huan-by-leetcode/ "Z 字形变换 | 题解（LeetCode）"
