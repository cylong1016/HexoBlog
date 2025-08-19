---
title: 哈希表统计与桶排序筛选前 K 个高频元素（LeetCode 347）
date: 2025-08-19 23:10:03
updated: 2025-08-19 23:10:03
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 数组
    - 分治
    - 堆
    - 哈希表
    - 桶
---
---

# 题目描述

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

你所设计算法的时间复杂度 **必须** 优于 `O(n log n)` ，其中 `n` 是数组大小。

**示例 1:**
> **输入：**`nums = [1, 1, 1, 2, 2, 3]`, `k = 2`
> **输出：**`[1, 2]`

**示例 2:**
> **输入：**`nums = [1]`, `k = 1`
> **输出：**`[1]`

**提示:**
* `1 <= nums.length <= 10^5`
* `k` 的取值范围是 `[1, 数组中不相同的元素的个数]`
* 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的

<!-- more -->

# 哈希表 + 频率映射

## 核心思路

使用哈希表统计频率，然后通过映射频率到数字列表，最后从高到低遍历频率；
1. **统计频率：**使用哈希表记录每个数字出现的次数。
2. **频率映射：**创建另一个哈希表，将频率作为键，对应的数字列表作为值。
3. **收集结果：**从最高频率（最大可能为数组长度）向下遍历，收集数字直到收集到 `k` 个。

## 代码实现

```java
public int[] topKFrequent(int[] nums, int k) {
    // 处理边界情况
    if (nums == null || nums.length == 0) {
        return new int[0];
    }

    // 步骤1：统计每个数字的出现次数
    Map<Integer, Integer> numToCountMap = new HashMap<>();
    for (int num : nums) {
        // 使用 merge 方法统计次数，等价于 getOrDefault 和 put
        // numToCountMap.put(num, numToCountMap.getOrDefault(num, 0) + 1);
        numToCountMap.merge(num, 1, Integer::sum);
    }

    // 步骤2：将频率映射到数字列表
    Map<Integer, List<Integer>> countToNumMap = new HashMap<>();
    for (Map.Entry<Integer, Integer> entry : numToCountMap.entrySet()) {
        int num = entry.getKey();
        int count = entry.getValue();
        // 如果该频率还没有列表，创建一个新列表
        if (!countToNumMap.containsKey(count)) {
            countToNumMap.put(count, new ArrayList<>());
        }
        // 将数字添加到对应频率的列表中
        countToNumMap.get(count).add(num);
    }

    // 步骤3：从最高频率开始收集数字
    List<Integer> res = new ArrayList<>();
    // 频率最大可能为数组长度，从高到低遍历
    for (int i = nums.length; i >= 0; i--) {
        if (countToNumMap.containsKey(i)) {
            List<Integer> numList = countToNumMap.get(i);
            for (Integer num : numList) {
                res.add(num);
                // 如果已收集到 k 个数字，转换为数组返回
                if (res.size() == k) {
                    return res.stream().mapToInt(Integer::intValue).toArray();
                }
            }
        }
    }

    return new int[0];
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，其中 `n` 是数组的长度。统计频率需要 `O(n)`，映射频率需要 `O(n)`，收集结果时最坏情况需要遍历所有频率（从 `n` 到 `0`），但每个数字只被处理一次，因此总体是 `O(n)`。
* **空间复杂度：**`O(n)`，需要两个哈希表来存储数字和频率的映射关系，最坏情况下每个数字的频率都不同，因此需要 `O(n)` 空间。

# 哈希表 + 桶排序

## 核心思路

将数字按频率分配到不同的桶中，然后从高频率桶开始收集元素。思路和方法一一致，这里其实只是把代码做了一些优化
1. **统计频率：**使用哈希表记录每个数字出现的次数。
2. **桶排序：**创建一个桶数组，索引表示频率，每个桶存储具有该频率的数字列表。（**与方法一区别：**`Map` 改为数组）
3. **收集结果：**从最高频率桶开始向下遍历，收集数字直到收集到 `k` 个。（**与方法一区别：** 频率最大由数组长度优化为统计后的最大值）

## 代码实现

```java
public int[] topKFrequent(int[] nums, int k) {
    // 处理边界情况
    if (nums == null || nums.length == 0) {
        return new int[0];
    }

    // 步骤1：统计每个数字的出现次数
    Map<Integer, Integer> numToCountMap = new HashMap<>();
    for (int num : nums) {
        // 使用 merge 方法统计次数，等价于 getOrDefault 和 put
        // numToCountMap.put(num, numToCountMap.getOrDefault(num, 0) + 1);
        // 1->3, 2->2, 3->1
        numToCountMap.merge(num, 1, Integer::sum);
    }

    // 步骤2：创建桶数组，索引代表频率
    int maxCnt = Collections.max(numToCountMap.values()); // 获取最大频率
    List<Integer>[] buckets = new ArrayList[maxCnt + 1];  // 桶数组，从 0 到 maxCnt
    Arrays.setAll(buckets, i -> new ArrayList<>());       // 初始化每个桶为空列表
    for (Map.Entry<Integer, Integer> entry : numToCountMap.entrySet()) {
        // 将数字添加到对应频率的桶中
        // 1->[3], 2->[2], 3->[1]
        buckets[entry.getValue()].add(entry.getKey());
    }

    // 步骤3：从最高频率桶开始收集数字
    int[] res = new int[k];
    for (int i = buckets.length - 1; i >= 0; i--) {
        for (Integer num : buckets[i]) {
            res[--k] = num;
            if (k == 0) {
                return res;
            }
        }
    }

    return new int[0];
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，其中 `n` 是数组的长度。统计频率需要 `O(n)`，创建桶需要 `O(n)`，收集结果需要 `O(n)`（因为每个数字只被处理一次）。
* **空间复杂度：**`O(n)`，需要哈希表存储频率，桶数组的空间最多为 `O(n)`。

# 总结

两种方法都是基于频率统计，但方法二使用桶排序更直观，效率也更高，因为避免了在方法一中可能的多余遍历。方法二的空间复杂度略高，但实际应用中差异不大。推荐使用方法二，代码更简洁且易于理解。

# 来源

> [347. 前 K 个高频元素 | 力扣（LeetCode）][1]
> [347. 前 K 个高频元素 | 题解 | 灵茶山艾府][2]

---

[1]: https://leetcode.cn/problems/top-k-frequent-elements/description/ "347. 前 K 个高频元素 | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/top-k-frequent-elements/solutions/3655287/tong-pai-xu-on-xian-xing-zuo-fa-pythonja-oqq2/ "347. 前 K 个高频元素 | 题解 | 灵茶山艾府"
