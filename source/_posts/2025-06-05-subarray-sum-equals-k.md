---
title: 和为 K 的子数组
date: 2025-06-05 23:43:23
updated: 2025-06-05 23:43:23
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode简单
    - Java
    - 学习笔记
    - 数组
    - 前缀和
    - 哈希表
---
---

# 题目描述

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 该数组中和为 `k` 的子数组的个数 。

**子数组** 是数组中元素的连续非空序列。

**示例 1:**
> **输入：**`nums = [1, 1, 1]`, `k = 2`
> **输出：**`2`

**示例 2:**
> **输入：**`nums = [1, 2, 3]`, `k = 3`
> **输出：**`2`


**提示:**
* `1 <= nums.length <= 2 * 10^4`
* `-1000 <= nums[i] <= 1000`
* `-10^7 <= k <= 10^7`

<!-- more -->

# 暴力枚举（双重循环）

最直观的方法是枚举所有可能的连续子数组，计算它们的和并统计等于 `k` 的个数。

```java
public int subarraySum(int[] nums, int k) {
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        int sum = 0;
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
            if (sum == k) {
                count++;
            }
        }
    }
    return count;
}
```

## 复杂度分析

* **时间复杂度：**`O(n²)`，其中 `n` 是数组长度。外层循环遍历每个元素作为起始点，内层循环遍历从起始点开始的所有子数组。
* **空间复杂度：**`O(1)`，只使用了常数级别的额外空间。

# 前缀和 + 哈希表

在做这道题之前，我们先看之前做过的两道题：

1. [303. 区域和检索 - 数组不可变 | 笑话人生][2]
2. [1. 两数之和 | 笑话人生][3]

## 思路

利用 **前缀和** 技术优化计算过程：
1. 计算数组的前缀和数组 `prefix`，其中：
    * `prefix[0] = 0`，表示空数组的和，用于处理从第一个元素开始的子数组
    * `prefix[i + 1] = nums[0] + nums[1] + ... + nums[i]（i ≥ 0）`
2. 对于任意子数组 `nums[i..j]`（包含下标 `i` 到 `j`），其和可表示为 `prefix[j + 1] - prefix[i]`
3. 问题转化为：寻找满足 `prefix[j + 1] - prefix[i] = k` 且 `0 ≤ i < j + 1 ≤ n` 的索引对 `(i, j)` 的数量
4. 使用哈希表存储每个前缀和出现的次数，遍历时查询 `prefix[j] - k` 的出现次数并累加（这其实就是上面的两数之和）

```java
public int subarraySum(int[] nums, int k) {
    int n = nums.length;
    int[] prefix = new int[n + 1];
    // 计算前缀和
    for (int i = 0; i < n; i++) {
        prefix[i + 1] = prefix[i] + nums[i];
    }

    int count = 0;
    // 设置容量可以快 2ms
    Map<Integer, Integer> countMap = new HashMap<>(n + 1);
    for (int p : prefix) {
        // 查找前缀和为 p - k 的出现次数
        count += countMap.getOrDefault(p - k, 0);
        // 更新当前前缀和的出现次数
        countMap.put(p, countMap.getOrDefault(p, 0) + 1);
    }
    return count;
}
```

## 示例解析
以 `nums = [1, 1, 1]`, `k=2` 为例：
1. 前缀和数组：`prefix = [0, 1, 2, 3]`
2. 遍历过程：
    * `p=0`：查找 `-2` → 不存在，`count=0`；更新 `map{0:1}`
    * `p=1`：查找 `-1` → 不存在，`count=0`；更新 `map{0:1, 1:1}`
    * `p=2`：查找 `0` → 存在1次，`count=1`；更新 `map{0:1, 1:1, 2:1}`
    * `p=3`：查找 `1` → 存在1次，`count=2`；更新 `map{0:1, 1:1, 2:1, 3:1}`
3. 返回结果：`2`

## 常见问题解答

1. 为什么需要 `prefix[0] = 0`？
考虑子数组从第一个元素开始的情况：子数组` nums[0..j]` 的和应为 `prefix[j + 1] - prefix[0]` 没有 `prefix[0]` 就无法表示从数组开头开始的子数组
2. 哈希表如何处理重复前缀和？当不同位置产生相同的前缀和时，哈希表记录出现次数。例如：
    * `nums = [0, 0, 0]`, `k=0`
    * 前缀和：`[0, 0, 0, 0]`
    * 遍历时：
        * `p=0`：查找 `0` → 初始不存在，`count=0`；记录 `0:1`
        * `p=0`：查找 `0` → 存在1次，`count=1`；记录 `0:2`
        * `p=0`：查找 `0` → 存在2次，`count=3`；记录 `0:3`
        * `p=0`：查找 `0` → 存在3次，`count=6`；记录 `0:4`
    * 返回结果：6（正确对应6个子数组）

## 复杂度分析

* **时间复杂度：**`O(n)`，其中 `n` 是数组长度。遍历数组两次（计算前缀和和统计子数组数量）。
* **空间复杂度：**`O(n)`，哈希表最多存储 `n + 1` 个键值对。

# 总结
1. 暴力枚举简单直接，适合小规模数据（`n ≤ 1000`）
2. 前缀和 + 哈希表是更高效的通用解法，时间复杂度 `O(n)`，能处理较大规模数据（`n ≤ 2×10⁴`）
3. 实际应用中优先选择方法二，其线性时间复杂度能有效应对常见场景
4. 关键点在于理解前缀和定义和索引映射关系：子数组和 = `prefix[j + 1] - prefix[i]`

# 来源

> [560. 和为 K 的子数组 | 力扣（LeetCode）][1]
> [560. 和为 K 的子数组 | 题解 | 灵茶山艾府][4]

---

[1]: https://leetcode.cn/problems/subarray-sum-equals-k/description/ "560. 和为 K 的子数组 | 力扣（LeetCode）"
[2]: /blog/2025/06/03/range-sum-query-immutable/ "303. 区域和检索 - 数组不可变 | 笑话人生"
[3]: /blog/2019/11/06/two-sum/ "1. 两数之和 | 笑话人生"
[4]: https://leetcode.cn/problems/subarray-sum-equals-k/solutions/1/qian-zhui-he-ha-xi-biao-cong-liang-ci-bi-4mwr/?envType=study-plan-v2&envId=top-100-liked "560. 和为 K 的子数组 | 题解 | 灵茶山艾府"
