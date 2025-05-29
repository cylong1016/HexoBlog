---
title: 连接两棵树后最大目标节点数目 Ⅰ
date: 2025-02-28 22:52:11
updated: 2025-02-28 22:52:11
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 树
    - 无向树
    - 深度优先搜索
---
---

# 题目描述

有两棵 `无向树`，分别有 `n` 和 `m` 个树节点。两棵树中的节点编号分别为 `[0, n - 1]` 和 `[0, m - 1]` 中的整数。

给你两个二维整数 `edges1` 和 `edges2` ，长度分别为 `n - 1` 和 `m - 1` ，其中 `edges1[i] = [ai, bi]` 表示第一棵树中节点 `ai` 和 `bi` 之间有一条边，`edges2[i] = [ui, vi]` 表示第二棵树中节点 `ui` 和 `vi` 之间有一条边。同时给你一个整数 `k` 。如果节点 `u` 和节点 `v` 之间路径的边数小于等于 `k` ，那么我们称节点 `u` 是节点 `v` 的 `目标节点` 。一个节点一定是它自己的 `目标节点` 。

请你返回一个长度为 `n` 的整数数组 `answer` ，`answer[i]` 表示将第一棵树中的一个节点与第二棵树中的一个节点连接一条边后，第一棵树中节点 `i` 的目标节点数目的最大值 。

**注意：**每个查询相互独立。意味着进行下一次查询之前，你需要先把刚添加的边给删掉。

**示例 1:**
> **输入：**`edges1 = [[0, 1], [0, 2], [2, 3], [2, 4]], edges2 = [[0, 1], [0, 2], [0, 3], [2, 7], [1, 4], [4, 5], [4, 6]]`, `k = 2`
> **输出：**`[9, 7, 9, 8, 8]`
> **解释：**
> * 对于 `i = 0` ，连接第一棵树中的节点 0 和第二棵树中的节点 0 。
> * 对于 `i = 1` ，连接第一棵树中的节点 1 和第二棵树中的节点 0 。
> * 对于 `i = 2` ，连接第一棵树中的节点 2 和第二棵树中的节点 4 。
> * 对于 `i = 3` ，连接第一棵树中的节点 3 和第二棵树中的节点 4 。
> * 对于 `i = 4` ，连接第一棵树中的节点 4 和第二棵树中的节点 4 。
{% asset_img 3372-1.png %}

**示例 2:**
> **输入：**`edges1 = [[0, 1], [0, 2], [0, 3], [0, 4]], edges2 = [[0, 1], [1, 2], [2, 3]]`, `k = 1`
> **输出：**`[6, 3, 3, 3, 3]`
> **解释：**对于每个 `i` ，连接第一棵树中的节点 `i` 和第二棵树中的任意一个节点。
{% asset_img 3372-2.png %}

**提示:**
* `2 <= n, m <= 1000`
* `edges1.length == n - 1`
* `edges2.length == m - 1`
* `edges1[i].length == edges2[i].length == 2`
* `edges1[i] = [ai, bi]`
* `0 <= ai, bi < n`
* `edges2[i] = [ui, vi]`
* `0 <= ui, vi < m`
* 输入保证 `edges1` 和 `edges2` 都表示合法的树。
* `0 <= k <= 1000`

<!-- more -->

# 思路分析

1. **第一棵树内部目标节点计算：**对于第一棵树中的每个节点 `i`，计算在不添加新边的情况下，距离 `i` 不超过 `k` 的节点数（包括自身）。这部分节点不受添加边的影响，因为新边连接的是另一棵树。
2. **第二棵树内部目标节点计算：**对于第二棵树中的每个节点 `j`，计算距离 `j` 不超过 `k-1` 的节点数（包括自身）。这是因为添加边后，从第一棵树节点 `i` 到第二棵树节点 `v` 的路径为 `i → a → b → v`，其中 `a` 和 `b` 是添加边的两个端点，路径长度为 `d1(i, a) + 1 + d2(b, v)`。为最大化第二棵树部分的贡献，选择 `a = i`（即连接点选在 `i`），这样路径长度简化为 `1 + d2(b, v)`。因此，第二棵树部分最多贡献距离 `b` 不超过 `k-1` 的节点数。
3. **最大化第二棵树贡献：**取第二棵树中所有节点在距离 `k-1` 内节点数的最大值 `maxCount2`。
4. **合并结果：**对于第一棵树中的每个节点 `i`，其目标节点最大值为 `count1[i] + maxCount2`，其中 `count1[i]` 是第一棵树内部距离 `i` 不超过 `k` 的节点数。

```java
public int[] maxTargetNodes(int[][] edges1, int[][] edges2, int k) {
    // 计算第一课无向树中每个节点的目标节点数
    int[] count1 = count(edges1, k);
    // 计算第一课无向树中每个节点的目标节点数
    int[] count2 = count(edges2, k - 1);
    // 计算第二课无向树中每个节点的目标节点数的最大值
    int maxCount2 = Arrays.stream(count2).max().orElse(0);

    int n = edges1.length + 1;
    int[] result = new int[n];
    for (int i = 0; i < n; i++) {
        result[i] = count1[i] + maxCount2;
    }
    return result;
}

public int[] count(int[][] edges, int k) {
    int n = edges.length + 1;
    // 构建无向树
    List<List<Integer>> graph = new ArrayList<>(n);
    for (int i = 0; i < n; i++) {
        graph.add(new ArrayList<>());
    }
    for (int[] edge : edges) {
        int u = edge[0], v = edge[1];
        graph.get(u).add(v);
        graph.get(v).add(u);
    }

    int[] result = new int[n];
    for (int i = 0; i < n; i++) {
        result[i] = dfs(i, -1, graph, k);
    }
    return result;
}

public int dfs(int u, int parent, List<List<Integer>> graph, int k) {
    if (k < 0) {
        return 0;
    }
    int count = 1;
    for (int v : graph.get(u)) {
        // 防止回溯到父节点，避免无限递归和重复计数
        if (v == parent) {
            continue;
        }
        count += dfs(v, u, graph, k - 1);
    }
    return count;
}
```

## 复杂度分析

* **时间复杂度：**`O(n² + m²)`，其中 `n` 和 `m` 分别是两棵树的节点数。计算 `count1` 和 `count2` 时，对每个节点进行 BFS，每次 BFS 最坏 `O(n)` 或 `O(m)`，总时间复杂度为 `O(n²)` 和 `O(m²)`。
* **空间复杂度：**`O(n + m)`，用于存储树的邻接表 `O(n + m)` 和 BFS 的队列及访问标记 `O(n)` 或 `O(m)`。

# 来源

> [3372. 连接两棵树后最大目标节点数目 Ⅰ | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/maximize-the-number-of-target-nodes-after-connecting-trees-i/description/ "3372. 连接两棵树后最大目标节点数目 Ⅰ | 力扣（LeetCode）"

