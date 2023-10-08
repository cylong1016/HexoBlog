---
title: 钥匙和房间
date: 2020-08-31 23:50:07
updated: 2020-08-31 23:50:07
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 图
    - 深度优先搜索
    - 广度优先搜索
    - 递归
    - 队列
---
---

# 题目描述

有 N 个房间，开始时你位于 0 号房间。每个房间有不同的号码：0，1，2，...，N - 1，并且房间里可能有一些钥匙能使你进入下一个房间。在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 rooms[i][j] 由 [0, 1，...，N - 1] 中的一个整数表示，其中 N = rooms.length。 钥匙 rooms[i][j] = v 可以打开编号为 v 的房间。

最初，除 0 号房间外的其余所有房间都被锁住。你可以自由地在房间之间来回走动。如果能进入每个房间返回 true，否则返回 false。

**示例 1：**
> 输入: [\[1\], \[2\], \[3\], []]
> 输出: true
> 解释:  
> 我们从 0 号房间开始，拿到钥匙 1。
> 之后我们去 1 号房间，拿到钥匙 2。
> 然后我们去 2 号房间，拿到钥匙 3。
> 最后我们去了 3 号房间。
> 由于我们能够进入每个房间，我们返回 true。

**示例 2：**
> 输入：[[1, 3], [3, 0, 1], \[2\], [0]]
> 输出：false
> 解释：我们不能进入 2 号房间。

**提示：**
* 1 <= rooms.length <= 1000
* 0 <= rooms[i].length <= 1000
* 所有房间中的钥匙数量总计不超过 3000。

<!-- more -->

# 深度优先搜索

此题我们将房间理解成节点，房间 A 到房间 B 理解成边，这样这道题就变成了，我们从图的 0 点出发，能否到达所有节点的问题。

具体实现上，我们使用一个变量 count 记录访问过的房间数，每次访问过一个房间后就标记为访问过 `visited[room] = true`，并将 `count++` ，如果最后 count 等于房间的数量，那么就说明可以访问所有的房间。

```java
int count = 0;
boolean[] visited;

public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    visited = new boolean[rooms.size()];
    visitRooms(rooms, 0);
    return count == rooms.size();
}

private void visitRooms(List<List<Integer>> rooms, Integer room) {
    visited[room] = true;
    count++;
    List<Integer> keyList = rooms.get(room);
    for (Integer nextRoom : keyList) {
        if (!visited[nextRoom]) {
            visitRooms(rooms, nextRoom);
        }
    }
}
```

## 复杂度分析

* 时间复杂度：O(n + m)，其中 n 是房间的数量，m 是所有房间中的钥匙数量的总数。
* 空间复杂度：O(n)，其中 n 是房间的数量。主要为栈空间的开销。

# 广度优先搜索

同样的，我们可以使用广度优先搜索解决此问题。

```java
public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    int n = rooms.size();
    int count = 0;
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<Integer>();
    visited[0] = true;
    queue.offer(0);
    while (!queue.isEmpty()) {
        int room = queue.poll();
        count++;
        for (int key : rooms.get(room)) {
            if (!visited[key]) {
                visited[key] = true;
                queue.offer(key);
            }
        }
    }
    return count == n;
}
```

## 复杂度分析

* 时间复杂度：O(n + m)，其中 n 是房间的数量，m 是所有房间中的钥匙数量的总数。
* 空间复杂度：O(n)，其中 n 是房间的数量。主要为队列的开销。

# 来源

> [钥匙和房间 | 力扣（LeetCode）][1]
> [钥匙和房间 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/keys-and-rooms/ "钥匙和房间 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/keys-and-rooms/solution/yao-chi-he-fang-jian-by-leetcode-solution/ "钥匙和房间 | 题解（LeetCode）"
