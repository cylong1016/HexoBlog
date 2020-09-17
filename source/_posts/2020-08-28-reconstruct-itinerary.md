---
title: 重新安排行程
date: 2020-08-28 23:10:29
updated: 2020-08-28 23:10:29
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 优先队列
    - 深度优先搜索
    - 回溯算法
    - 图
    - 欧拉路径
    - 递归
---
---

# 题目描述

给定一个机票的字符串二维数组 [from, to]，子数组中的两个成员分别表示飞机出发和降落的机场地点，对该行程进行重新规划排序。所有这些机票都属于一个从 JFK（肯尼迪国际机场）出发的先生，所以该行程必须从 JFK 开始。

**提示：**

1. 如果存在多种有效的行程，请你按字符自然排序返回最小的行程组合。例如，行程 ["JFK", "LGA"] 与 ["JFK", "LGB"] 相比就更小，排序更靠前。
2. 所有的机场都用三个大写字母表示（机场代码）。
3. 假定所有机票至少存在一种合理的行程。
4. 所有的机票必须都用一次 且 只能用一次。

**示例 1：**
> 输入：[["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
> 输出：["JFK", "MUC", "LHR", "SFO", "SJC"]

**示例 2：**
> 输入：[["JFK", "SFO"],["JFK", "ATL"],["SFO", "ATL"],["ATL", "JFK"],["ATL", "SFO"]]
> 输出：["JFK", "ATL", "JFK", "SFO", "ATL", "SFO"]
> 解释：另一种有效的行程是 ["JFK", "SFO", "ATL", "JFK", "ATL", "SFO"]。但是它自然排序更大更靠后。

<!-- more -->

# 题解

本题是一道求解欧拉回路/欧拉通路的问题。也叫「一笔画」问题，下面给出定义。

* 通过图中所有边恰好一次且行遍所有顶点的通路称为欧拉通路。
* 通过图中所有边恰好一次且行遍所有顶点的回路称为欧拉回路。
* 具有欧拉回路的无向图称为欧拉图。
* 具有欧拉通路但不具有欧拉回路的无向图称为半欧拉图。

因为本题保证至少存在一种合理的路径，也就告诉了我们，这张图是一个欧拉图或者半欧拉图。我们只需要输出这条欧拉通路的路径即可。如果没有保证至少存在一种合理的路径，我们需要判别这张图是否是欧拉图或者半欧拉图，具体地：

* 对于无向图 G，G 是欧拉图当且仅当 G 是连通的且没有奇度顶点。
* 对于无向图 G，G 是半欧拉图当且仅当 G 是连通的且 G 中恰有 2 个奇度顶点。
* 对于有向图 G，G 是欧拉图当且仅当 G 的所有顶点属于同一个强连通分量且每个顶点的入度和出度相同。
* 对于有向图 G，G 是半欧拉图当且仅当 G 的所有顶点属于同一个强连通分量且。
 * 恰有一个顶点的出度与入度差为 1；
 * 恰有一个顶点的入度与出度差为 1；
 * 所有其他顶点的入度和出度相同。
 
接下来我们考虑如下的行程：合法路径为 JFK→BBB→JFK→AAA

{% asset_img 欧拉通路.png 欧拉通路 %}

算法 Hierholzer 算法用于在连通图中寻找欧拉路径，其流程如下：

1. 从起点出发，进行深度优先搜索。
2. 每次沿着某条边从某个顶点移动到另外一个顶点的时候，都需要删除这条边。
3. 如果没有可移动的路径，则将所在节点加入到栈中，并返回。

当我们顺序地考虑该问题时，我们也许很难解决该问题，根据上图我们可以发现，如果我们先走到 AAA 的顶点，就回不去了，我们走入了「死胡同」，从而导致无法遍历到其他还未访问的边。于是我们希望能够遍历完当前节点所连接的其他节点后再进入「死胡同」。

> 注意对于每一个节点，它只有最多一个「死胡同」分支。依据前言中对于半欧拉图的描述，只有那个入度与出度差为 1 的节点会导致死胡同。

不妨倒过来思考。我们注意到只有那个入度与出度差为 1 的节点会导致死胡同。而该节点必然是最后一个遍历到的节点。我们可以改变入栈的规则，当我们遍历完一个节点所连的所有节点后，我们才将该节点入栈（即逆序入栈）。对于当前节点而言，从它的每一个非「死胡同」分支出发进行深度优先搜索，都将会搜回到当前节点。而从它的「死胡同」分支出发进行深度优先搜索将不会搜回到当前节点。也就是说当前节点的死胡同分支将会优先于其他非「死胡同」分支入栈。另外为了保证我们能够快速找到当前节点所连的节点中字典序最小的那一个，我们可以使用优先队列存储当前节点所连到的点。

这样就能保证我们可以「一笔画」地走完所有边，最终的栈中逆序地保存了「一笔画」的结果。我们只要将栈中的内容反转，即可得到答案。

```java
Map<String, PriorityQueue<String>> graph = new HashMap<>();
List<String> ans = new LinkedList<>();

public List<String> findItinerary(List<List<String>> tickets) {

    for (List<String> ticket : tickets) {
        if (graph.containsKey(ticket.get(0))) {
            graph.get(ticket.get(0)).add(ticket.get(1));
        } else {
            PriorityQueue<String> queue = new PriorityQueue<>();
            queue.add(ticket.get(1));
            graph.put(ticket.get(0), queue);
        }
    }
    buildEulerPath("JFK");
    Collections.reverse(ans);
    return ans;
}

public void buildEulerPath(String travel) {
    while (graph.containsKey(travel) && graph.get(travel).size() > 0) {
        String tmpTravel = graph.get(travel).poll();
        buildEulerPath(tmpTravel);
    }
    ans.add(travel);
}
```

## 复杂度分析

* 时间复杂度：O(mlogm)，其中 m 是边的数量。对于每一条边我们需要 O(logm) 地删除它，最终的答案序列长度为 m + 1。
* 空间复杂度：O(m)，其中 m 是边的数量。我们需要存储每一条边。

# 来源
> [重新安排行程 | 力扣（LeetCode）][1]
> [重新安排行程 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/reconstruct-itinerary/ "重新安排行程 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/reconstruct-itinerary/solution/zhong-xin-an-pai-xing-cheng-by-leetcode-solution/ "重新安排行程 | 题解（LeetCode）"