---
title: 动态规划求解单词拆分问题（LeetCode 139）
date: 2025-09-05 23:38:11
updated: 2025-09-05 23:38:11
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 动态规划
    - 记忆化搜索
    - 递归
    - 数组
---
---

# 题目描述

给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 `s` 则返回 `true`。

**注意：**不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

**示例 1:**
> **输入：**`s = "leetcode"`, `wordDict = ["leet", "code"]`
> **输出：**`true`
> **解释：**返回 `true` 因为 `leetcode` 可以由 `leet` 和 `code` 拼接成。

**示例 2:**
> **输入：**`s = "applepenapple"`, `wordDict = ["apple", "pen"]`
> **输出：**`true`
> **解释：**返回 `true` 因为 `applepenapple` 可以由 `apple` 、 `pen` 和 `apple` 拼接成。

**示例 3:**
> **输入：**`s = "catsandog"`, `wordDict = ["cats", "dog", "sand", "and", "cat"]`
> **输出：**`false`

**提示:**
* `1 <= s.length <= 300`
* `1 <= wordDict.length <= 1000`
* `1 <= wordDict[i].length <= 20`
* `s` 和 `wordDict[i]` 仅由小写英文字母组成
* `wordDict` 中的所有字符串互不相同

<!-- more -->

# 动态规划

我们可以使用动态规划来解决这个问题。定义 `dp[i]` 表示字符串 `s` 的前 `i` 个字符（即子串 `s[0:i-1]`）是否可以被拆分成字典中的单词。

## 算法步骤

1. 将单词字典转换为哈希集合，提高查找效率
2. 初始化 `dp[0] = true`，表示空字符串可以被拆分
3. 对于每个位置 `i`（从 `1` 到字符串长度），检查所有可能的分割点 `j`（从 `0` 到 `i-1`）
4. 如果 `dp[j]` 为真且子串 `s[j:i]` 在字典中，则设置 `dp[i] = true`
5. 最终 `dp[s.length()]` 就是整个字符串是否可以被拆分的答案

## 代码实现

```java
public boolean wordBreak(String s, List<String> wordDict) {
    // 将单词字典转换为哈希集合，提高查找效率
    Set<String> wordDictSet = new HashSet<>(wordDict);
    
    // 创建 dp 数组，dp[i] 表示 s 的前 i 个字符是否可以被拆分
    boolean[] dp = new boolean[s.length() + 1];
    
    // 初始化：空字符串可以被拆分
    dp[0] = true;
    
    // 遍历字符串的每个位置
    for (int i = 1; i <= s.length(); i++) {
        // 检查所有可能的分割点
        for (int j = 0; j < i; j++) {
            // 如果前 j 个字符可以被拆分，且子串 s[j:i] 在字典中
            if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                // 设置 dp[i] 为 true，并跳出内层循环
                dp[i] = true;
                break;
            }
        }
    }
    
    // 返回整个字符串是否可以被拆分
    return dp[s.length()];
}
```

## 动态规划过程详解

以 `s = "leetcode"`, `wordDict = ["leet", "code"]` 为例，详细说明动态规划每一步的计算过程：

1. 初始化：`dp[0] = true`（空字符串可以被拆分）。
2. i=1：检查前 1 个字符 "l"。
    * j=0：子串 "l" 不在字典中，`dp[1] = false`。
3. i=2：检查前 2 个字符 "le"。
    * j=0：子串 "le" 不在字典中。
    * j=1：子串 "e" 不在字典中，`dp[2] = false`。
4. i=3：检查前 3 个字符 "lee"。
    * j=0：子串 "lee" 不在字典中。
    * j=1：子串 "ee" 不在字典中。
    * j=2：子串 "e" 不在字典中，`dp[3] = false`。
5. i=4：检查前 4 个字符 "leet"。
    * j=0：子串 "leet" 在字典中，且 `dp[0]=true`，所以 `dp[4] = true`（跳出内层循环）。
6. i=5：检查前 5 个字符 "leetc"。
    * j=0：子串 "leetc" 不在字典中。
    * j=1：子串 "eetc" 不在字典中。
    * j=2：子串 "etc" 不在字典中。
    * j=3：子串 "tc" 不在字典中。
    * j=4：子串 "c" 不在字典中（注意：这里检查的是从索引 4 到 5 的子串 "c"，因为 `s.substring(4,5)` 返回 "c"），所以` dp[5] = false`。
7. i=6：检查前 6 个字符 "leetco"。
    * 类似地，检查所有分割点，没有找到满足条件的子串，`dp[6] = false`。
8. i=7：检查前 7 个字符 "leetcod"。
    * 没有满足条件的子串，`dp[7] = false`。
9. i=8：检查前 8 个字符 "leetcode"。
    * j=0：子串 "leetcode" 不在字典中。
    * j=1：子串 "eetcode" 不在字典中。
    * ... 其他分割点也不满足。
    * j=4：子串 "code" 在字典中，且 `dp[4]=true`，所以 `dp[8] = true`。
9. 最终返回 `dp[8]=true`，表示 "leetcode" 可以被拆分为 "leet" 和 "code"。

## 复杂度分析

* **时间复杂度：**`O(n²)`，其中 `n` 是字符串的长度。因为有两层循环，外层循环遍历每个字符，内层循环最多遍历 `i` 次
* **空间复杂度：**`O(n + m)`，其中 `n` 是字符串长度（`dp` 数组大小），`m` 是字典中的单词数量（哈希集合大小）

# 剪枝优化

为了提升性能，我们引入两种剪枝优化：

1. **最大长度剪枝：**
    * 通过计算字典中单词的最大长度，我们避免了检查那些长度超过最大单词长度的子串，这可以显著减少内层循环的次数。
    * 例如，当检查位置i=5时，我们只需要从j=1开始检查，而不是从j=0开始。
2. **从后向前检查：**
    * 通过从后向前检查分割点，我们优先检查较短的子串，这样一旦找到匹配的单词就可以提前退出循环，减少不必要的检查。
    * 在实际应用中，较短的单词通常更常见，因此这种检查顺序可以提高效率。

## 代码实现

```java
public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordDictSet = new HashSet<>(wordDict);
    
    // 计算字典中单词的最大长度
    int maxWordLength = 0;
    for (String word : wordDict) {
        maxWordLength = Math.max(maxWordLength, word.length());
    }
    
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;
    
    for (int i = 1; i <= s.length(); i++) {
        // 优化 1：只检查可能的分割点
        // 计算起始检查位置，避免检查长度超过最大单词长度的子串
        int start = Math.max(0, i - maxWordLength);
        // 优化 2：从后向前检查，优先匹配较短的单词
        // 从 i-1 开始向前检查，一旦找到匹配的单词就可以提前退出循环
        for (int j = i - 1; j >= start; j--) {
            if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[s.length()];
}
```

# 总结

单词拆分问题是一个经典的动态规划问题，通过合理的剪枝优化，我们可以显著提高算法的性能。本文介绍了两种有效的优化技巧：长度剪枝和检查顺序优化，并通过代码示例展示了如何实现这些优化。在实际应用中，根据具体情况选择合适的优化策略，可以在保证正确性的同时提高算法的执行效率。

# 来源

> [139. 单词拆分 | 力扣（LeetCode）][1]
> [139. 单词拆分 | 题解 | 力扣官方题解][2]
> [139. 单词拆分 | 题解 | 灵茶山艾府][3]

---

[1]: https://leetcode.cn/problems/word-break/description/ "139. 单词拆分 | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/word-break/solutions/302471/dan-ci-chai-fen-by-leetcode-solution/ "139. 单词拆分 | 题解 | 力扣官方题解"
[3]: https://leetcode.cn/problems/word-break/solutions/2968135/jiao-ni-yi-bu-bu-si-kao-dpcong-ji-yi-hua-chrs/ "139. 单词拆分 | 题解 | 灵茶山艾府"
