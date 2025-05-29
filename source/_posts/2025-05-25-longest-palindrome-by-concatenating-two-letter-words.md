---
title: 连接两字母单词得到的最长回文串
date: 2025-05-25 21:31:32
updated: 2025-05-25 21:31:32
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 贪心算法
---
---

# 题目描述

给你一个字符串数组 `words` 。`words` 中每个元素都是一个包含两个小写英文字母的单词。请你从 `words` 中选择一些元素并按任意顺序连接它们，并得到一个尽可能长的回文串 。每个元素至多只能使用一次。请你返回你能得到的最长回文串的长度 。如果没办法得到任何一个回文串，请你返回 `0` 。

**说明:**
`回文串` 指的是从前往后和从后往前读一样的字符串。

**示例 1:**
> **输入：**`words = ["lc", "cl", "gg"]`
> **输出：**6
> **解释：**一个最长的回文串为 `"lc" + "gg" + "cl" = "lcggcl"` ，长度为 6 。`"clgglc"` 是另一个可以得到的最长回文串。

**示例 2:**
> **输入：**`words = ["ab", "ty", "yt", "lc", "cl", "ab"]`
> **输出：**8
> **解释：**最长回文串是 `"ty" + "lc" + "cl" + "yt" = "tylcclyt"` ，长度为 8 。`"lcyttycl"` 是另一个可以得到的最长回文串。

**示例 3:**
> **输入：**`words = ["cc", "ll", "xx"]`
> **输出：**2
> **解释：**最长回文串是 `"cc"` ，长度为 2 。`"ll"` 是另一个可以得到的最长回文串。`"xx"` 也是。

**提示:**
* `1 <= words.length <= 10^5`
* `words[i].length == 2`
* `words[i]` 仅包含小写英文字母。

<!-- more -->

# 思路分析

1. 统计单词出现次数：使用哈希表记录每个单词的出现次数。
2. 处理互为反转的单词对：对于每个单词，检查其反转是否存在。若存在，取两者出现次数的较小值，每对贡献 `4` 个字符（每个单词长度为 `2` ）。
3. 处理对称单词：对于两个字符相同的单词（如 `"aa"`），可以成对使用（贡献 `4` 个字符），若有剩余且未使用中间点，可单独作为中间对称点（贡献 `2` 个字符）。
4. 避免重复处理：使用集合记录已处理的单词，确保每对单词只处理一次。

```java
public int longestPalindrome(String[] words) {
    // 记录最长回文长度
    int maxLen = 0;
    // 标记是否已使用中间对称点
    boolean center = false;
    // 记录已处理的单词
    List<String> processed = new ArrayList<>();
    // 使用 HashMap 统计每个单词的出现次数
    Map<String, Integer> wordCount = new HashMap<>();
    // 记录所有 word 的次数
    Arrays.stream(words).forEach(word -> wordCount.put(word, wordCount.getOrDefault(word, 0) + 1));

    for (String word : wordCount.keySet()) {
        if (processed.contains(word)) {
            continue;
        }
        int count = wordCount.get(word);
        String reversed = new StringBuilder(word).reverse().toString();
        // 若单词字符相同（如"aa"），计算可成对使用的次数，剩余次数作为中间对称点。
        if (word.equals(reversed)) {
            int pairs = count / 2;
            maxLen += pairs * 4;
            if (!center && count % 2 == 1) {
                maxLen += 2;
                center = true;
            }
            processed.add(word);
        } else {
            // 找到反转单词，取较小次数计算贡献，避免重复处理。
            if (wordCount.containsKey(reversed)) {
                int reversedCount = wordCount.get(reversed);
                int pairs = Math.min(count, reversedCount);
                maxLen += pairs * 4;
                processed.add(word);
                processed.add(reversed);
            }

        }
    }
    return maxLen;
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，遍历 `words` 数组并构建哈希表 `wordCount`，时间复杂度为 `O(n)`，遍历哈希表的键集合，假设不同的单词数量为 `m`，每个单词的处理包括生成反转字符串、哈希表查询等操作。由于每个单词长度为 `2`，生成反转字符串和哈希表操作均为 `O(1)`，总时间复杂度为 `O(m)`。最坏情况下，所有单词均不同（即 `m = n`），时间复杂度为 `O(n)`。其中 `n` 是数组长度。
* **空间复杂度：**`O(n)`，`wordCount` 存储所有单词及其频率，占用 `O(m)` 空间。`processed` 集合存储已处理的单词，需要 `O(m)` 空间。总体空间复杂度：`O(m)`，即 `O(n)`（`m` 最大为 `n`）。

# 来源

> [2131. 连接两字母单词得到的最长回文串 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/longest-palindrome-by-concatenating-two-letter-words/description/ "2131. 连接两字母单词得到的最长回文串 | 力扣（LeetCode）"
