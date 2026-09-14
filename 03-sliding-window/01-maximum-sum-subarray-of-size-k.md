---
title: Maximum Sum Subarray of Size K
difficulty: Easy
leetcode_title: Maximum Average Subarray I
leetcode: https://leetcode.com/problems/maximum-average-subarray-i/
tags:
  - Array
  - Sliding Window
---

# Maximum Sum Subarray of Size K

## Problem description

Given an array of positive numbers and a positive number `k`, find the maximum sum of any contiguous subarray of size `k`.

## Examples

**Example 1:**

```plaintext
Input: [2, 1, 5, 1, 3, 2], k = 3
Output: 9
Explanation: Subarray with maximum sum is [5, 1, 3].
```

**Example 2:**

```plaintext
Input: [2, 3, 4, 1, 5], k = 2
Output: 7
Explanation: Subarray with maximum sum is [3, 4].
```

## Constraints

- `1 <= k <= len(nums)`
- All numbers in `nums` are positive

## Hints

<details>
<summary>Hint 1</summary>

Write out the sums of the first two windows side by side. How many of the numbers being added are the same in both?

</details>

<details>
<summary>Hint 2</summary>

Moving the window one step right removes exactly one value and adds exactly one value. You never need to revisit the `k - 2` values in the middle.

</details>

## Solution 1: Brute force

### Intuition

Every window of size `k` starts at some index `i`, so try them all and add up the `k` values each time.

### Algorithm

1. For each start index `i` from `0` to `len(nums) - k`
2. Sum the `k` values beginning at `i`
3. Keep the largest sum seen

### Complexity analysis

- Time complexity: $O(n \cdot k)$ — there are $n - k + 1$ windows and each one is summed from scratch in $k$ steps.
- Space complexity: $O(1)$ — only the running maximum is stored.

```python
from typing import List


class Solution:
    def find_max_sum(self, nums: List[int], k: int) -> int:
        maximum_sum = 0

        for start in range(len(nums) - k + 1):
            window_sum = sum(nums[start:start + k])
            maximum_sum = max(maximum_sum, window_sum)

        return maximum_sum
```

## Solution 2: Sliding window

### Intuition

Neighbouring windows overlap in `k - 1` values, so the brute-force solution re-adds almost the same numbers on every iteration. Carry the sum forward instead: when the window slides one step, subtract the value that fell off the left and add the value that arrived on the right.

```plaintext
index      0   1   2   3   4   5        k = 3
value      2   1   5   1   3   2

           └───────┘                    sum = 8
               └───────┘                sum = 8 - 2 + 1 = 7
                   └───────┘            sum = 7 - 1 + 3 = 9   ← maximum
                       └───────┘        sum = 9 - 5 + 2 = 6
```

### Algorithm

1. Walk `window_end` across the array, adding each value to `window_sum`
2. Once `window_end` reaches index `k - 1` the window holds exactly `k` values, so record the sum
3. Subtract `nums[window_start]` and advance `window_start`, restoring the window to `k - 1` values ready for the next arrival

### Complexity analysis

- Time complexity: $O(n)$ — each value is added once and subtracted at most once.
- Space complexity: $O(1)$ — only two integers are kept alongside the boundaries.

```python
from typing import List


class Solution:
    def find_max_sum(self, nums: List[int], k: int) -> int:
        maximum_sum = 0

        window_start = 0
        window_sum = 0
        for window_end in range(len(nums)):
            window_sum += nums[window_end]

            # the window is exactly k wide, so it is a candidate
            if window_end >= k - 1:
                maximum_sum = max(maximum_sum, window_sum)
                window_sum -= nums[window_start]
                window_start += 1

        return maximum_sum
```

Seeding `maximum_sum` with `0` is safe here only because the problem guarantees positive numbers. See [Maximum Average Subarray I](./01.1-maximum-average-subarray-i.md), where that assumption is dropped.

## Relationship to [Maximum Average Subarray I](./01.1-maximum-average-subarray-i.md)

That problem is the LeetCode form of this one: the same fixed window, the same running sum, the same slide. Only two things change — it divides the sum by `k` before recording, and it allows negative numbers, which breaks the `0` seed used above.
