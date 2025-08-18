---
title: 基于分治思想的第 K 大元素查找实现（LeetCode 215）
date: 2025-08-18 23:14:00
updated: 2025-08-18 23:14:00
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
    - 双指针
---
---

# 题目描述

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `k` 个最大的元素。请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1:**
> **输入：**`[3, 2, 1, 5, 6, 4]`, `k = 2`
> **输出：**`5`
**示例 2:**
> **输入：**`[3, 2, 3, 1, 2, 4, 5, 5, 6]`, `k = 4`
> **输出：**`4`

**提示:**
* `1 <= k <= nums.length <= 10^5`
* `-10^4 <= nums[i] <= 10^4`

<!-- more -->

# 直接排序

## 核心思路

将数组升序排序后，第 `k` 大的元素位于索引 `nums.length - k` 处。

## 代码实现

```java
import java.util.Arrays;

public int findKthLargest(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    Arrays.sort(nums);            // 升序排序
    return nums[nums.length - k]; // 返回倒数第k个元素
}
```

## 复杂度分析

* **时间复杂度：**`O(n log n)`，由排序算法决定。
* **空间复杂度：**`O(1)`（忽略排序的栈空间开销）。

# 快速选择（分治）

## 核心思路

利用快速排序的分区思想：
1. 随机选择基准值 `pivot`。
2. 将数组分为三部分：`big`（大于 `pivot`）、`small`（小于 `pivot`）和等于 `pivot` 的元素。
3. 根据 `k` 与 `big.size()` 的关系递归处理：
    * 若 `k <= big.size()`：第 `k` 大元素在 `big` 中。
    * 若 `k > nums.size() - small.size()`：第 `k` 大元素在 `small` 中（需调整 `k` 值）。
    * 否则，`pivot` 即为目标值。

## 代码实现

```java
import java.util.*;

public int findKthLargest(int[] nums, int k) {
    List<Integer> numList = new ArrayList<>();
    for (int num : nums) {
        numList.add(num);
    }
    return quickSelect(numList, k);
}

private int quickSelect(List<Integer> nums, int k) {
    // 随机选择基准值
    Random rand = new Random();
    int pivot = nums.get(rand.nextInt(nums.size()));
    
    // 划分大于、小于基准值的元素
    List<Integer> big = new ArrayList<>();
    List<Integer> small = new ArrayList<>();
    for (int num : nums) {
        if (num > pivot) {
            big.add(num);
        } else if (num < pivot) {
            small.add(num();
        }
    }
    
    if (k <= big.size()) {
        // 在 big 中递归查找
        return quickSelect(big, k);
    } else if (nums.size() - small.size() < k) {
        // 调整 k 值后在 small 中查找
        return quickSelect(small, k - (nums.size() - small.size()));
    }
    // pivot 是第 k 大元素
    return pivot;
}
```

## 复杂度分析

* **时间复杂度：**平均 `O(n)`，最坏 `O(n²)`（随机化避免最坏情况）。
* **空间复杂度：**`O(n)`（递归栈和列表存储）。

# 原地快速选择（空间优化）

## 核心思路

在方法二基础上优化空间：
1. 目标位置 `target = nums.length - k`（升序后第 `target` 个元素）。
2. 分区时随机选择基准值并交换到末尾。
3. 使用双指针分区：
    * `i` 标记小于基准值的边界。
    * 遍历数组，将小于基准值的元素交换到 `i` 左侧。
4. 根据分区结果递归左/右子数组。

## 代码实现

```java
import java.util.Random;

public int findKthLargest(int[] nums, int k) {
    int n = nums.length;
    int target = n - k; // 升序排序后第 target 个元素是第 k 大
    return quickSelect(nums, 0, n - 1, target);
}

private int quickSelect(int[] nums, int left, int right, int target) {
    // 递归终止
    if (left == right) {
        return nums[left];
    }
    
    // 随机选择基准值并交换到末尾
    Random rand = new Random();
    int pivotIdx = left + rand.nextInt(right - left + 1);
    swap(nums, pivotIdx, right);
    int pivot = nums[right];
    
    // 分区操作
    int i = left; // i 左侧元素均 < pivot
    for (int j = left; j < right; j++) {
        if (nums[j] < pivot) {
            swap(nums, i, j);
            i++;
        }
    }
    swap(nums, i, right); // 将基准值放到正确位置
    
    if (i == target) {
        return nums[i]; // 找到目标
    } else if (i < target) {
        return quickSelect(nums, i + 1, right, target); // 递归右子数组
    } else {
        return quickSelect(nums, left, i - 1, target); // 递归左子数组
    }
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

## 复杂度分析

* **时间复杂度：**平均 `O(n)`，最坏 `O(n²)`（随机化避免最坏情况）。
* **空间复杂度：**`O(1)`（原地分区），递归栈平均 `O(log n)`。

# 总结

| 解法                  | 时间复杂度   | 空间复杂度  | 适用场景        |
| -------------------- | ----------- | --------- | -------------- |
| 直接排序               | O(n log n) |  O(1)      | 快速实现       |
| 快速选择（分治）        | 平均 O(n)   |  O(n)     | 无需严格空间约束 |
| 原地快速选择           | 平均 O(n)   |  O(log n)  | 空间效率最优    |

实际应用中，原地快速选择综合性能最佳，尤其适合处理大规模数据。

# 来源

> [215. 数组中的第K个最大元素 | 力扣（LeetCode）][1]
> [215. 数组中的第K个最大元素 | 题解 | Krahets][2]

---

[1]: https://leetcode.cn/problems/kth-largest-element-in-an-array/description/ "215. 数组中的第K个最大元素 | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/kth-largest-element-in-an-array/solutions/2361969/215-shu-zu-zhong-de-di-k-ge-zui-da-yuan-d786p/ "215. 数组中的第K个最大元素 | 题解 | Krahets"
