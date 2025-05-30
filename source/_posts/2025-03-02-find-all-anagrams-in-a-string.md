---
title: 找到字符串中所有字母异位词
date: 2025-03-02 23:00:44
updated: 2025-03-02 23:00:44
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 字符串
    - 哈希表
    - 滑动窗口
---
---

# 题目描述

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 `异位词` 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

`字母异位词` 是通过重新排列不同单词或短语的字母而形成的单词或短语，并使用所有原字母一次。

**示例 1:**
> **输入：**`s = "cbaebabacd"`, `p = "abc"`
> **输出：**`[0, 6]`
> **解释:**
> 起始索引等于 `0` 的子串是 `"cba"`, 它是 `"abc"` 的异位词。
> 起始索引等于 `6` 的子串是 `"bac"`, 它是 `"abc"` 的异位词。

**示例 2:**
> **输入：**`s = "abab"`, `p = "ab"`
> **输出：**`[0, 1, 2]`
> **解释:**
> 起始索引等于 `0` 的子串是 `"ab"`, 它是 `"ab"` 的异位词。
> 起始索引等于 `1` 的子串是 `"ba"`, 它是 `"ab"` 的异位词。
> 起始索引等于 `2` 的子串是 `"ab"`, 它是 `"ab"` 的异位词。

**提示:**
* `1 <= s.length`, `p.length <= 3 * 10^4`
* `s` 和 `p` 仅包含小写字母

<!-- more -->

# 固定窗口滑动 + 计数数组

1. **初始化处理：**
* 若 `s` 长度小于 `p`，直接返回空列表。
* 创建两个长度为 `26` 的数组 `pCount` 和 `sCount`，分别统计 `p` 的字符频次和 `s` 中前 `p.length()` 个字符的频次。
2. **首次匹配检查：**
* 比较 `pCount` 和 `sCount`，若相等说明起始索引 `0` 是异位词，加入结果列表。
3. **滑动窗口：**
* 窗口长度固定为 `p.length()`，每次右移一位：
    * 移除窗口最左侧字符（对应频次减 `1`）。
    * 加入窗口右侧新字符（对应频次加 `1`）。
* 每次窗口移动后，比较 `pCount` 和 `sCount`，若相等则记录当前窗口起始索引（`i + 1`）。

```java
public List<Integer> findAnagrams(String s, String p) {
    if (s == null || p == null) {
        return new ArrayList<>();
    }

    int sLen = s.length();
    int pLen = p.length();
    if (sLen < pLen) {
        return new ArrayList<>();
    }

    // 统计 p 和 s 的前 pLen 个字符出现的次数，如果相等，则说明是异位词
    int[] pCount = new int[26];
    int[] sCount = new int[26];
    for (int i = 0; i < pLen; i++) {
        pCount[p.charAt(i) - 'a']++;
        sCount[s.charAt(i) - 'a']++;
    }

    List<Integer> anagrams = new ArrayList<>();
    if (Arrays.equals(pCount, sCount)) {
        anagrams.add(0);
    }

    // 定长滑动窗口，每次移动一个字符，判断是否是异位词
    for (int i = 0; i < s.length() - p.length(); i++) {
        sCount[s.charAt(i) - 'a']--;
        sCount[s.charAt(i + p.length()) - 'a']++;
        if (Arrays.equals(pCount, sCount)) {
            anagrams.add(i + 1);
        }
    }

    return anagrams;
}
```

## 复杂度分析

* **时间复杂度：**`O(26n)`，其中 `n` 是 `s` 的长度。因为每次窗口移动后，我们都要比较两个长度为 `26` 的数组（`Arrays.equals` 内部会循环 `26` 次）。
* **空间复杂度：**`O(1)`，固定使用两个长度为 `26` 的数组（常数空间）。

# 不固定窗口滑动 + 计数数组

以下参考大佬的解法：[438. 找到字符串中所有字母异位词 | 题解 | 灵茶山艾府][2]

1. **频次数组预处理：**
* 统计 `p` 的字符频次到数组 `pCount`。
2. **双指针滑动窗口：**
* 使用双指针 `left` 和 `right`，`right` 向右移动，将遇到的字符在 `pCount` 中减 `1`（相当于进入窗口）。
* 如果某个字符在 `pCount` 中的值小于 `0`，说明当前窗口中这个字符的数量超过了 `p` 中该字符的数量，或者 `p` 中根本没有这个字符。那么就需要移动 `left` 指针，将 `left` 指向的字符在 `pCount` 中加 `1`（相当于移出窗口），直到 `pCount[c]` 不再小于 `0`（即调整到当前字符数量正常）。
* 当窗口长度（`right - left + 1`）等于 `p` 的长度时，说明我们找到了一个异位词子串，将 `left` 加入结果列表。

**注意：**这种方法中，`pCount` 数组被复用，我们通过加减操作来维护窗口内字符的计数。当窗口长度等于 `p` 的长度时，由于我们保证了窗口内每个字符的出现次数都不超过 `p` 中的出现次数（通过 `while` 循环调整），并且窗口长度恰好等于 `p` 的长度，那么窗口内的字符串必然是 `p` 的一个异位词。因为如果窗口内某个字符的出现次数大于 `p` 中的出现次数，我们会通过 `left` 右移来减少它，直到它等于 `p` 中的出现次数（即不再为负）。而当窗口长度等于 `p` 的长度时，每个字符的出现次数恰好等于 `p` 中的出现次数（因为如果少了，那么 `pCount` 中对应的值应该是正数，但我们的操作中，进入窗口减 `1`，移出窗口加 `1`，并且我们保证了没有负值，所以每个字符都不多不少）。

```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> anagrams = new ArrayList<>();
    // 统计 p 的每种字母的出现次数
    int[] pCount = new int[26];
    for (char c : p.toCharArray()) {
        pCount[c - 'a']++;
    }
    int left = 0;
    for (int right = 0; right < s.length(); right++) {
        int c = s.charAt(right) - 'a';
        // 右端点字母进入窗口
        pCount[c]--;
        // 字母 c 太多了，left右移，直到窗口中字母 c 的出现次数为 0
        while (pCount[c] < 0) {
            pCount[s.charAt(left) - 'a']++;
            left++;
        }
        // s' 和 p 的每种字母的出现次数都相同
        if (right - left + 1 == p.length()) {
            // s' 左端点下标加入答案
            anagrams.add(left);
        }
    }
    return anagrams;
}
```

## 复杂度分析

* **时间复杂度：**`O(m + n)`，其中 `m` 是 `s` 的长度，`n` 是 `p` 的长度。虽然写了个二重循环，但是内层循环中对 `left` 加一的总执行次数不会超过 `m` 次，所以滑窗的时间复杂度为 `O(m)`。
* **空间复杂度：**`O(1)`，使用固定大小的数组（ `26` 个整数）。

# 来源

> [438. 找到字符串中所有字母异位词 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/find-all-anagrams-in-a-string/description/ "438. 找到字符串中所有字母异位词 | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/find-all-anagrams-in-a-string/solutions/1/liang-chong-fang-fa-ding-chang-hua-chuan-14pd/ "438. 找到字符串中所有字母异位词 | 题解 | 灵茶山艾府"
