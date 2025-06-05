---
title: 字母异位词分组
date: 2025-05-30 00:28:11
updated: 2025-05-30 00:28:11
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 数组
    - 哈希表
    - 字符串
    - 排序
---
---

# 题目描述

给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的所有字母得到的一个新单词。

**示例 1:**
> **输入：**`strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`
> **输出：**`[["bat"], ["nat", "tan"], ["ate", "eat", "tea"]]`

**示例 2:**
> **输入：**`strs = [""]`
> **输出：**`[[""]]`

**示例 3:**
> **输入：**`strs = ["a"]`
> **输出：**`[["a"]]`

**提示:**
* `1 <= strs.length <= 104`
* `0 <= strs[i].length <= 100`
* `strs[i]` 仅包含小写字母

<!-- more -->

# 排序 + 哈希表

字母异位词指字母相同，但排列不同的字符串。所以对每个字符串排序，将排序后的字符串作为 `key`，原字符串添加到该 `key` 对应的列表中。（如 `"eat"` 和 `"tea"` 的 `key` 都为 `"aet"`）。

```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> resultMap = new HashMap<>();
    for (String str : strs) {
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);
        if (!resultMap.containsKey(key)) {
            resultMap.put(key, new ArrayList<>());
        }
        resultMap.get(key).add(str);
    }
    return new ArrayList<>(resultMap.values());
}
```

## 复杂度分析

* **时间复杂度：**`O(nk logk)`，假设有 `n` 个字符串，每个字符串最大长度为 `k`。排序每个字符串的时间复杂度为`O(k logk)`，所以总时间复杂度为`O(nk logk)`。
* **空间复杂度：**`O(nk)`，哈希表存储所有字符串（最坏情况无重复 `key`）。

# 字符计数 + 哈希表

字母异位词指字母相同，但排列不同的字符串，所以，每个字符串里字母的数量是相等的，故可以统计每个字符串中每个字符出现的次数，然后生成一个表示字符频次的字符串作为 `key`（例如：`"a1b2"`），然后将原字符串添加到该 `key` 对应的列表中。

```java
public List<List<String>> groupAnagrams(String[] strs) {
    int[] count = new int[26];
    Map<String, List<String>> resultMap = new HashMap<>();
    for (String str : strs) {
        Arrays.fill(count, 0);
        for (char c : str.toCharArray()) {
            // 统计每个字符出现的次数
            count[c - 'a']++;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 26; i++) {
            // 拼接每个字符出现的次数，作为 Map 的 key
            if (count[i] > 0) {
                sb.append((char) ('a' + i));
                sb.append(count[i]);
            }
        }
        String key = sb.toString();
        if (!resultMap.containsKey(key)) {
            resultMap.put(key, new ArrayList<>());
        }
        resultMap.get(key).add(str);
    }
    // 入参：["eatt", "teat", "tan", "atet", "nat", "bat"]
    // resultMap 结果为：{a1b1t1=[bat], a1n1t1=[tan, nat], a1e1t2=[eatt, teat, atet]}
    return new ArrayList<>(resultMap.values());
}
```

## 复杂度分析

* **时间复杂度：**`O(nk)`，哈希表存储所有字符串（最坏情况无重复 key）。
* **空间复杂度：**`O(1)`。

# 关键结论

* **排序 + 哈希表：**代码简洁，但排序开销随字符串变长而增大。
* **计数 + 哈希表：**避免排序，通过字符频次编码更高效处理长字符串。
* **实际选择：**若字符串平均长度小，选方法一；若长度大，选方法二。

通过哈希表将字母异位词映射到相同 `key`，两种方法均能高效解决分组问题！

# 来源

> [49. 字母异位词分组 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/group-anagrams/description/ "49. 字母异位词分组 | 力扣（LeetCode）"
