---
title: 全排列问题的递归回溯解法实现（LeetCode 46）
date: 2025-07-15 22:54:28
updated: 2025-07-15 22:54:28
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 数组
    - 回溯
    - 深度优先搜索
    - 剪枝
---
---

# 题目描述

给定一个不含重复数字的数组 `nums` ，返回其所有可能的全排列 。你可以按任意顺序返回答案。

**示例 1:**
> **输入：**`nums = [1, 2, 3]`
> **输出：**`[[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]`

**示例 2:**
> **输入：**`nums = [0, 1]`
> **输出：**`[[0, 1], [1, 0]]`

**示例 3:**
> **输入：**`nums = [1]`
> **输出：**`[[1]]`

**提示:**
* `1 <= nums.length <= 6`
* `-10 <= nums[i] <= 10`
* `nums` 中的所有整数互不相同

<!-- more -->

# 回溯算法（DFS）

## 核心思路

全排列问题可以通过回溯算法解决。核心思想是：每次从数组中选取一个未被使用的元素加入当前路径，当路径长度等于数组长度时，将当前路径加入结果集。之后通过回溯撤销选择，尝试其他可能的元素。

1. 初始化结果列表 `ans` 和当前路径列表 `t`
2. 创建布尔数组 `vis` 标记元素是否已被使用
3. 使用深度优先搜索（DFS）进行回溯：
    * **终止条件：**当前路径长度等于数组长度，将路径拷贝加入结果集
    * **遍历选择：**遍历数组中的每个元素：
        * 如果元素未被使用，则将其加入路径并标记为已使用
        * 递归进入下一层决策树
        * 回溯：移除路径最后一个元素并取消标记

## 代码实现

```java
public static List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    List<Integer> t = new ArrayList<>();
    boolean[] vis = new boolean[nums.length];
    dfs(nums, ans, t, vis);
    return ans;
}

public static void dfs(int[] nums, List<List<Integer>> ans, List<Integer> t, boolean[] vis) {
    // 当路径长度等于数组长度时，将当前路径加入结果集
    if (t.size() == nums.length) {
        // 注意创建新列表
        ans.add(new ArrayList<>(t));
        return;
    }

    // 遍历所有元素
    for (int i = 0; i < nums.length; i++) {
        // 如果元素未被使用
        if (!vis[i]) {
            // 做出选择
            t.add(nums[i]);
            vis[i] = true;

            // 递归进入下一层
            dfs(nums, ans, t, vis);

            // 撤销选择（回溯）
            t.removeLast();
            vis[i] = false;
        }
    }
}
```

## 关键步骤图示

**初始状态**
```
Level 0: 
  t = [] 
  vis = [false, false, false]
```
**第一层递归（选择第一个元素）**
```
选择1:
  t = [1]
  vis = [true, false, false]
  └─ 进入第二层递归
```
**第二层递归（选择第二个元素）**
```
选择2:
  t = [1, 2]
  vis = [true, true, false]
  └─ 进入第三层递归
```
**第三层递归（选择第三个元素）**
```
选择3:
  t = [1, 2, 3] → 添加到结果集 ✅
  vis = [true, true, true]
  ├─ 回溯：移除3
  │   t = [1, 2]
  │   vis = [true, true, false]
  └─ 回溯：移除2
      t = [1]
      vis = [true, false, false]
```
**第二层递归的其他选择**
```
选择3:
  t = [1, 3]
  vis = [true, false, true]
  └─ 进入第三层递归
      选择2:
        t = [1, 3, 2] → 添加到结果集 ✅
        vis = [true, true, true]
        ├─ 回溯：移除2
        │   t = [1, 3]
        │   vis = [true, false, true]
        └─ 回溯：移除3
            t = [1]
            vis = [true, false, false]
```
**第一层递归的其他选择**
```
选择2:
  t = [2]
  vis = [false, true, false]
  └─ 进入第二层递归
      选择1:
        t = [2, 1]
        vis = [true, true, false]
        └─ 进入第三层递归
            选择3:
              t = [2, 1, 3] → 添加到结果集 ✅
              vis = [true, true, true]
      选择3:
        t = [2, 3]
        vis = [false, true, true]
        └─ 进入第三层递归
            选择1:
              t = [2, 3, 1] → 添加到结果集 ✅
              vis = [true, true, true]

选择3:
  t = [3]
  vis = [false, false, true]
  └─ 进入第二层递归
      选择1:
        t = [3, 1]
        vis = [true, false, true]
        └─ 进入第三层递归
            选择2:
              t = [3, 1, 2] → 添加到结果集 ✅
              vis = [true, true, true]
      选择2:
        t = [3, 2]
        vis = [false, true, true]
        └─ 进入第三层递归
            选择1:
              t = [3, 2, 1] → 添加到结果集 ✅
              vis = [true, true, true]
```

## 复杂度分析

* **时间复杂度：**`O(n*n!)`，共有 `n!` 个排列，每个排列需要 `O(n)` 时间复制到结果列表中。
* **空间复杂度：**`O(n)`，递归栈深度为 `n`，标记数组 `vis` 占用 `O(n)` 空间（结果空间不计入复杂度分析）。

# 关键点总结

1. **回溯模板：**遵循"选择-递归-撤销"的标准回溯框架
2. **路径拷贝：**`ans.add(new ArrayList<>(t))` 创建新列表避免引用问题
3. **状态标记：**使用 `vis` 数组高效判断元素是否可用
4. **去重处理：**题目已说明数组无重复数字，无需额外去重逻辑

回溯法是解决排列组合问题的经典方法，通过深度优先搜索遍历所有可能性，配合状态标记保证每个元素只使用一次。掌握回溯算法的核心框架和剪枝技巧，能高效解决此类问题。

# 来源

> [46. 全排列 | 力扣（LeetCode）][1]
> [46. 全排列 | 题解 | liweiwei1419][2]
> [46. 全排列 | 题解 | Krahets][3]

---

[1]: https://leetcode.cn/problems/permutations/description/ "46. 全排列 | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/permutations/solutions/9914/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/ "46. 全排列 | 题解 | liweiwei1419"
[3]: https://leetcode.cn/problems/permutations/solutions/2363882/46-quan-pai-lie-hui-su-qing-xi-tu-jie-by-6o7h/ "46. 全排列 | 题解 | Krahets"
