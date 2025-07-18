---
title: 双指针法原地移动零元素（LeetCode 283）
date: 2025-03-01 21:59:48
updated: 2025-03-01 21:59:48
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode简单
    - Java
    - 学习笔记
    - 数组
    - 双指针
---
---

# 题目描述

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意，**必须在不复制数组的情况下原地对数组进行操作。

**示例 1:**
> **输入：**`nums = [0, 1, 0, 3, 12]`
> **输出：**`[1, 3, 12, 0, 0]`

**示例 2:**
> **输入：**`nums = [0]`
> **输出：**`[0]`

**提示:**
* `1 <= nums.length <= 10^4`
* `-2^31 <= nums[i] <= 2^31 - 1`

<!-- more -->

# 双指针（交换法）

使用两个指针，`left` 和 `right`，`right` 指针用于遍历数组，`left` 指针用于指向当前已经处理好的序列的尾部（即非零序列的末尾，也就是下一个非零元素要放置的位置）。当 `right` 指针遇到非零元素时，就将其与 `left` 指针指向的元素交换，然后 `left` 指针右移。这样，非零元素被逐渐交换到前面，而 `0` 被交换到后面。

**注意：**交换后，`left` 指向的位置可能是 `0`（如果之前 `left` 指向的是 `0`）或者非零（如果 `left` 和 `right` 相同，即自己交换自己，此时不会改变），但无论如何，`left` 指针的左侧都是非零元素，并且保持了原有顺序。

```java
public void moveZeroes(int[] nums) {
    int left = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] != 0) {
            // 避免不必要的自交换
            if (left != right) {
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
            }
            left++;
        }
    }
    // 数组变化过程
    // 输入：[0, 1, 0, 3, 12]
    // [0, 1, 0, 3, 12]
    // [1, 0, 0, 3, 12]
    // [1, 0, 0, 3, 12]
    // [1, 3, 0, 0, 12]
    // [1, 3, 12, 0, 0]
}
```

执行过程（以 `[0, 1, 0, 3, 12]` 为例）
`[0, 1, 0, 3, 12]` - `right=0: 0` → 跳过
`[1, 0, 0, 3, 12]` - `right=1: 1` → 与 `left(0)` 交换
`[1, 0, 0, 3, 12]` - `right=2: 0` → 跳过
`[1, 3, 0, 0, 12]` - `right=3: 3` → 与 `left(1)` 交换
`[1, 3, 12, 0, 0]` - `right=4: 12` → 与 `left(2)` 交换

## 复杂度分析

* **时间复杂度：**`O(n)`，只需一次遍历数组。
* **空间复杂度：**`O(1)`，原地操作，仅使用常数空间。

# 双指针（覆盖法）

使用一个指针 `cur`，表示当前非零元素应该存放的位置。遍历数组，当遇到非零元素时，将其复制到 `cur` 位置，然后 `cur` 指针右移。遍历完成后，所有非零元素都被按顺序移动到了数组的前部，然后将 `cur` 之后的元素全部置为 `0`。

```java
public void moveZeroes(int[] nums) {
    int cur = 0;
    // 第一阶段：移动非零元素
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[cur++] = nums[i];
        }
    }
    // 第二阶段：填充零
    while (cur < nums.length) {
        nums[cur++] = 0;
    }
    // 数组变化过程
    // 输入：[0, 1, 0, 3, 12]
    // 输入：[0, 1, 0, 3, 12]
    // [0, 1, 0, 3, 12]
    // [1, 1, 0, 3, 12]
    // [1, 1, 0, 3, 12]
    // [1, 3, 0, 3, 12]
    // [1, 3, 12, 3, 12]
    // [1, 3, 12, 0, 0]
}
```

执行过程（以 `[0, 1, 0, 3, 12]` 为例）

**第一阶段：**
* `i=0: 0` → 跳过
* `i=1: 1` → `nums[0]=1` (`cur=1`)
* `i=2: 0` → 跳过
* `i=3: 3` → `nums[1]=3` (`cur=2`)
* `i=4: 12` → `nums[2]=12` (`cur=3`)

**第二阶段：**
* 填充 `nums[3]=0`, `nums[4]=0`
* 结果: `[1, 3, 12, 0, 0]`

## 复杂度分析

* **时间复杂度：**`O(n)`，两次独立遍历数组。
* **空间复杂度：**`O(1)`，原地操作，仅使用常数空间。

# 总结

两种方法都有效地解决了移动零的问题：

1. 交换法更高效，单次遍历完成操作，代码更简洁
2. 覆盖法写操作更少，但需要两次遍历

在实际应用中，交换法通常是更优选择，因为它只需要一次遍历且代码更简洁。理解这两种双指针策略有助于解决类似的数组重排问题，如移除指定元素、删除排序数组中的重复项等。

# 来源

> [283. 移动零 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/move-zeroes/description/ "283. 移动零 | 力扣（LeetCode）"
