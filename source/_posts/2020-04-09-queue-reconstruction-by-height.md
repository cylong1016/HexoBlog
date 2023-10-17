---
title: 根据身高重建队列
date: 2020-04-09 12:48:12
updated: 2020-04-09 12:48:12
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 贪心算法
    - 排序
    - 链表
    - 数组
---
---

# 题目描述

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对(h, k)表示，其中h是这个人的身高，k是排在这个人前面且身高大于或等于h的人数。 编写一个算法来重建这个队列。

**注意：** 总人数少于1100人。

**示例**
> 输入: [[7, 0], [4, 4], [7, 1], [5, 0], [6, 1], [5, 2]]
> 输出: [[5, 0], [7, 0], [5, 2], [6, 1], [4, 4], [7, 1]]

<!-- more -->

# 题解

让我们从最简单的情况下思考，当队列中所有人的 (h, k) 都是相同的高度 h，只有 k 不同时，解决方案很简单：每个人在队列的索引 index = k。

{% asset_img 所有人等高.jpg 所有人等高的情况 %}

即使不是所有人都是同一高度，这个策略也是可行的。因为个子矮的人相对于个子高的人是 “看不见” 的，所以可以先安排个子高的人。下图中我们先安排身高为 7 的人，将它放置在与 k 值相等的索引上；再安排身高为 6 的人，同样的将它放置在与 k 值相等的索引上。

{% asset_img 不等高.jpg 不等高的情况 %}

该策略可以递归进行：
* 将最高的人按照 k 值升序排序，然后将它们放置到输出队列中与 k 值相等的索引位置上。
* 按降序取下一个高度，同样按 k 值对该身高的人升序排序，然后逐个插入到输出队列中与 k 值相等的索引位置上。
* 直到完成为止。

```java
public int[][] reconstructQueue(int[][] people) {
    // 按照高度降序排列，同高度的人，按照k值升序排列。
    Arrays.sort(people, (o1, o2) -> o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0]);
    LinkedList<int[]> list = new LinkedList<>();
    // 逐个地把它们放在输出队列中，索引等于它们的 k 值。
    for (int[] person : people) {
        list.add(person[1], person);
    }
    return list.toArray(new int[list.size()][]);
}
```

## 复杂度分析

* 时间复杂度：O(N²)，排序使用了 O(NlogN) 的时间，每个人插入到输出队列中需要 O(k) 的时间，其中 k 是当前输出队列的元素个数。总共的时间复杂度为O(N²)。
* 空间复杂度：O(N)，输出队列使用的空间。

# 来源

> [根据身高重建队列 | 力扣（LeetCode）][1]
> [根据身高重建队列 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/queue-reconstruction-by-height/ "根据身高重建队列 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/gen-ju-shen-gao-zhong-jian-dui-lie-by-leetcode/ "根据身高重建队列 | 题解（LeetCode）"
