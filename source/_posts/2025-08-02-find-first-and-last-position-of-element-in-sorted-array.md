---
title: 两次二分查找定位有序数组元素边界（LeetCode 34）
date: 2025-08-02 14:22:23
updated: 2025-08-02 14:22:23
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 数组
    - 二分查找
---
---

# 题目描述

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1:**
> **输入：**`nums = [5, 7, 7, 8, 8, 10]`, `target = 8`
> **输出：**`[3, 4]`

**示例 2:**
> **输入：**`nums = [5, 7, 7, 8, 8, 10]`, `target = 6`
> **输出：**`[-1, -1]`

**示例 3:**
> **输入：**`nums = []`, `target = 0`
> **输出：**`[-1, -1]`

**提示:**
* `0 <= nums.length <= 10^5`
* `-10^9 <= nums[i] <= 10^9`
* `nums` 是一个非递减数组
* `-10^9 <= target <= 10^9`

<!-- more -->

# 一次二分查找 + 线性扩展

## 核心思路

使用二分查找找到目标值，然后向左右两边扩展，直到找到边界。

## 代码实现

```java
public int[] searchRange(int[] nums, int target) {
    // 处理空数组的情况
    if (nums == null || nums.length == 0) {
        return new int[]{-1, -1};
    }
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        // 计算中间位置，防止整数溢出
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            // 找到目标值，继续向两边搜索边界
            int start = mid, end = mid;
            // 向左搜索起始位置
            while (start > left && nums[start - 1] == target) {
                start--;
            }
            // 向右搜索结束位置
            while (end < right && nums[end + 1] == target) {
                end++;
            }
            return new int[] {start, end};
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return new int[] {-1, -1};
}
```

## 复杂度分析

* **时间复杂度：**平均情况 `O(logn)`，但在最坏情况下（整个数组都是目标值）会退化为 `O(n)`。
* **空间复杂度：**`O(1)`，仅使用常数空间存储变量。

# 两次二分查找

## 核心思路

分别使用二分查找寻找左边界和右边界：
* **查找左边界：**第一个等于目标值的位置
    * 初始化左右指针 `left = 0`, `right = nums.length - 1`
    * 当 `left <= right` 时循环：
        * 计算中间位置 `mid = left + (right - left) / 2`（防止整数溢出）
        * 如果 `nums[mid] >= target`，则目标可能在左半部分，调整右指针 `right = mid - 1`
        * 否则调整左指针 `left = mid + 1`
    * 循环结束后，检查 `left` 是否在数组范围内且对应值等于 `target`
* **查找右边界：**最后一个等于目标值的位置
    * 初始化左右指针 `left = 0`, `right = nums.length - 1`
    * 当 `left <= right` 时循环：
        * 计算中间位置 `mid = left + (right - left) / 2`（防止整数溢出）
        * 如果 `nums[mid] <= target`，则目标可能在右半部分，调整左指针 `left = mid + 1`
        * 否则调整右指针 `right = mid - 1`
    * 循环结束后，检查 `right` 是否在数组范围内且对应值等于 `target`

## 代码实现

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        // 处理空数组的情况
        if (nums == null || nums.length == 0) {
            return new int[]{-1, -1};
        }
        
        int leftBound = findLeftBound(nums, target);
        int rightBound = findRightBound(nums, target);
        return new int[]{leftBound, rightBound};
    }
    
    // 查找左边界：第一个等于 target 的位置
    private int findLeftBound(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) {
                right = mid - 1; // 向左搜索
            } else {
                left = mid + 1;
            }
        }
        // 检查 left 是否在数组范围内且等于 target
        if (left < nums.length && nums[left] == target) {
            return left;
        }
        return -1;
    }
    
    // 查找右边界：最后一个等于 target 的位置
    private int findRightBound(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] <= target) {
                left = mid + 1; // 向右搜索
            } else {
                right = mid - 1;
            }
        }
        // 检查 right 是否在数组范围内且等于 target
        if (right >= 0 && nums[right] == target) {
            return right;
        }
        return -1;
    }
}
```

## 复杂度分析

* **时间复杂度：**`O(log n)`。两次二分查找，每次时间复杂度为 `O(log n)`。
* **空间复杂度：**`O(1)`。仅使用常数空间存储变量。

# 总结

| 解法                  | 时间复杂度                   | 空间复杂度  | 优点                                              |
| -------------------- | --------------------------- | --------- | ------------------------------------------------ |
| 一次二分查找 + 线性扩展 | O(n)（最坏）<br>O(log n)（最好）  | O(1)      | 代码简单，但是受重复元素数量影响，最坏情况时间复杂度 O(n) |
| 两次二分查找           | O(log n)（所有情况）          | O(1)      | 保证最坏情况下的性能，不受输入数据分布影响               |

最终推荐两次二分查找方法，它严格满足题目要求，在各种情况下都能保持高效稳定的性能，是符合题目要求的解法。

# 来源

> [34. 在排序数组中查找元素的第一个和最后一个位置 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/ "34. 在排序数组中查找元素的第一个和最后一个位置 | 力扣（LeetCode）"
