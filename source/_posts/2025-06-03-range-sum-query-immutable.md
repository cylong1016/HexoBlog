---
title: 区域和检索 - 数组不可变
date: 2025-06-03 23:20:55
updated: 2025-06-03 23:20:55
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode简单
    - Java
    - 学习笔记
    - 数组
    - 前缀和
---
---

# 题目描述

给定一个整数数组  `nums`，处理以下类型的多个查询:

计算索引 `left` 和 `right` （包含 `left` 和 `right`）之间的 `nums` 元素的和 ，其中 `left <= right`。

实现 `NumArray` 类：

* `NumArray(int[] nums)` 使用数组 `nums` 初始化对象
* `int sumRange(int i, int j)` 返回数组 `nums` 中索引 `left` 和 `right` 之间的元素的总和 ，包含 `left` 和 `right` 两点（也就是 `nums[left] + nums[left + 1] + ... + nums[right]` )

**示例 1:**
> **输入：**
> `["NumArray", "sumRange", "sumRange", "sumRange"]`
> `[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]`
> **输出：**
> `[null, 1, -1, -3]`
> **解释：**
> `NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);`
> `numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)`
> `numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) `
> `numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))`

**提示:**
* `1 <= nums.length <= 10^4`
* `-10^5 <= nums[i] <= 10^5`
* `0 <= i <= j < nums.length`
* 最多调用 `10*4` 次 `sumRange` 方法

<!-- more -->

# 直接计算

最简单的思路，每次调用 `sumRange()` 时，直接遍历 `left` 到 `right` 的元素并求和。

```java
class NumArray {

    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }
    
    public int sumRange(int left, int right) {
        int sum = 0;
        for (int i = left; i <= right; i++) {
            sum += nums[i];
        }
        return sum;
    }
}
```

## 复杂度分析

* **时间复杂度：**`O(mn)`，其中 `m` 为查询次数，`n` 为数组长度。
* **空间复杂度：**`O(1)`，不考虑存储输入的数组。

# 前缀和

前缀和是一种高效处理区间求和问题的预处理技术，其核心思想是通过数学变换将区间求和转化为两个端点的差值计算。下面我们通过数学推导详细解释前缀和的工作原理。

## 步骤1：定义前缀和数组

给定数组 `nums[0...n-1]`，我们定义前缀和数组 `prefix`：

> `prefix[0] = 0`
> `prefix[1] = nums[0]`
> `prefix[2] = nums[0] + nums[1]`
> ...
> `prefix[i] = nums[0] + nums[1] + ... + nums[i-1]`
> ...
> `prefix[n] = nums[0] + ... + nums[n-1]`

## 步骤2：区间和与前缀和的数学关系

考虑区间 `[left, right]` 的和：

> `sum(left, right)`
> `= nums[left] + nums[left+1] + ... + nums[right]`
> `= (nums[0] + ... + nums[left-1] + nums[left] + ... + nums[right]) - (nums[0] + ... + nums[left-1])`
> `= prefix[right+1] - prefix[left]`

```java
class NumArray {

    private int[] prefix;

    public NumArray(int[] nums) {
        // 多一位存储0位置
        prefix = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            // 递推关系：当前前缀和 = 前一个前缀和 + 当前元素
            prefix[i + 1] = prefix[i] + nums[i];
        }
    }
    
    public int sumRange(int left, int right) {
        return prefix[right + 1] - prefix[left];
    }
}
```

## 前缀和的核心优势
1. **查询高效化：**将 `O(n)` 的求和操作转化为 `O(1)` 的差值计算。
2. **预处理思想：**一次构建多次使用，特别适合查询密集型场景。
3. **数学简洁性：**通过简单的数组差值实现复杂区间计算。

## 复杂度分析

* **时间复杂度：**`O(n)`，初始化前缀和数组，其中 `n` 是数组长度。`sumRange` 是 `O(1)`。
* **空间复杂度：**`O(n)`，存储前缀和数组。

# 来源

> [303. 区域和检索 - 数组不可变 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/range-sum-query-immutable/description/ "303. 区域和检索 - 数组不可变 | 力扣（LeetCode）"
