---
title: 组合总和问题：回溯与剪枝策略应用（面试题）
date: 2025-08-24 22:08:40
updated: 2025-08-24 22:08:40
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 前缀和
    - 后缀和
    - 数组
    - 递归
    - 剪枝
    - 回溯
    - 深度优先搜索
    - 面试
---
---

# 题目描述

提供一个整数数组 `nums`，从中选 `m` 个数，打印所有和为 `n` 的 二维数组，注意兼顾性能。

**示例 1:**
> **输入：**`nums = [-1, 1, 2, 3, 4, 5, 6]`, `m = 2`, `n = 5`
> **输出：**`[[1, 4], [2, 3], [-1, 6]]`

**示例 2:**
> **输入：**`nums = [-1, 1, 2, 3, 4, 5, 6]`, `m = 3`, `n = 6`
> **输出：**`[[-1, 1, 6], [-1, 2, 5], [-1, 3, 4], [1, 2, 3]]`

<!-- more -->

# 递归回溯 + 剪枝

## 核心思路

我们需要从一个整数数组中找到所有长度为 `m` 且和为 `n` 的组合。为了兼顾性能，我们采用回溯法并结合剪枝策略来减少不必要的计算。具体步骤如下：

1. **排序数组：**首先对数组进行排序，这样可以方便地跳过重复元素，并利用有序性进行剪枝优化。
2. **预处理：**
    * **前缀和数组：**计算前缀和数组 `prefixSum`，`prefixSum[i]` 表示前 `i` 个元素的和，用于计算任意连续子数组的和。前缀和参考：[前缀和解法实现区间和查询（LeetCode 303） | 笑话人生][1]
    * **后缀和数组：**计算后缀和数组 `suffixSum`，`suffixSum[k]` 表示后 `k` 个元素的和，用于计算剩余元素的最大可能和。
3. **回溯搜索：**使用回溯方法生成所有可能的组合。在每一步中，检查当前组合的和是否可能达到目标值，通过以下剪枝条件提前终止不必要的分支：
    * 当前和加上剩余元素的最大可能和仍小于目标值。
    * 当前和加上剩余元素的最小可能和已超过目标值。
    * 当前元素加上当前和已超过目标值（由于数组已排序，后续元素只会更大）。
    * 跳过重复元素，避免生成重复的组合。

## 代码实现

```java
public void combinationSum(int[] nums, int m, int n) {
    if (nums == null || nums.length < m) {
        System.out.println(Arrays.toString(new int[0]));
        return;
    }

    // 对数组进行排序
    Arrays.sort(nums);

    int len = nums.length;
    // 计算前缀和数组，其中 prefixSum[i] 表示前 i 个数字的和
    // [0, -1, 0, 2, 5, 9, 14, 20]
    int[] prefixSum = new int[len + 1];
    for (int i = 0; i < len; i++) {
        prefixSum[i + 1] = prefixSum[i] + nums[i];
    }
    // 计算后缀和数组，其中 suffixSum[k] 表示最后 k 个数字的和
    // [0, 6, 11, 15, 18, 20, 21, 20]
    int[] suffixSum = new int[m + 1];
    for (int k = 1; k <= m; k++) {
        suffixSum[k] = prefixSum[len] - prefixSum[len - k];
    }

    List<List<Integer>> results = new ArrayList<>();
    List<Integer> current = new ArrayList<>();
    backtrack(nums, m, n, 0, current, 0, results, prefixSum, suffixSum);
    // 打印结果
    System.out.println(results);
}

private void backtrack(int[] nums, int m, int n, int start, List<Integer> current, int currentSum,
                       List<List<Integer>> results, int[] prefixSum, int[] suffixSum) {
    // 还需要选择的数字个数
    int r = m - current.size();
    if (r == 0) {
        if (currentSum == n) {
            results.add(new ArrayList<>(current));
        }
        return;
    }
    // 检查剩余数字是否足够
    if (start + r > nums.length) {
        return;
    }
    // 计算从 start 开始 r 个数字的最小和
    int minSumFromStart = prefixSum[start + r] - prefixSum[start];
    // 如果当前和加上剩余元素的最小可能和已超过目标值，直接返回。
    if (currentSum + minSumFromStart > n) {
        return;
    }
    // 如果当前和加上剩余元素的最大可能和仍小于目标值，直接返回。
    if (currentSum + suffixSum[r] < n) {
        return;
    }
    for (int i = start; i < nums.length; i++) {
        // 跳过重复元素，避免重复组合
        if (i > start && nums[i] == nums[i - 1]) {
            continue;
        }

        // 因为数组已排序，如果当前数字加上当前和已经大于n，后面的数字更大，直接跳出循环
        if (currentSum + nums[i] > n) {
            break;
        }

        current.add(nums[i]);
        backtrack(nums, m, n, i + 1, current, currentSum + nums[i], results, prefixSum, suffixSum);
        // 回溯
        current.remove(current.size() - 1);
    }
}
```

## 复杂度分析

* **时间复杂度：**排序数组的时间复杂度为 `O(l log l)` 其中 `l` 是数组长度。回溯的时间复杂度最坏情况下为 `O(C(l, m))`，即组合数，但通过剪枝策略，实际运行时间会大大减少。
* **空间复杂度：**`O(m)` 用于递归调用栈和存储当前组合，`O(l)` 用于前缀和数组，`O(m)` 用于后缀和数组。

# 总结

通过排序数组、计算前缀和与后缀和，并结合回溯与剪枝策略，我们能够高效地解决组合求和问题。这种方法不仅确保了正确性，还通过剪枝显著提高了性能，适用于处理较大的输入数组。

---

[1]: /blog/2025/06/03/range-sum-query-immutable/ "前缀和解法实现区间和查询（LeetCode 303） | 笑话人生"
