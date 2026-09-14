---
title: Smallest Subarray with a Given Sum
difficulty: Medium
leetcode_title: Minimum Size Subarray Sum
leetcode: https://leetcode.com/problems/minimum-size-subarray-sum/
tags:
  - Array
  - Binary Search
  - Sliding Window
  - Prefix Sum
---

# Smallest Subarray with a Given Sum

## Problem description

Given an array of positive numbers and a positive number `target`, find the length of the smallest contiguous subarray whose sum is greater than or equal to `target`. Return `0` if no such subarray exists.

## Examples

**Example 1:**

```plaintext
Input: [2, 1, 5, 2, 3, 2], target = 7
Output: 2
Explanation: The smallest subarray with a sum greater than or equal to '7' is [5, 2].
```

**Example 2:**

```plaintext
Input: [2, 1, 5, 2, 8], target = 7
Output: 1
Explanation: The smallest subarray with a sum greater than or equal to '7' is [8].
```

**Example 3:**

```plaintext
Input: [3, 4, 1, 1, 6], target = 8
Output: 3
Explanation: Smallest subarrays with a sum greater than or equal to '8' are [3, 4, 1] or [1, 1, 6].
```

## Constraints

- `1 <= target <= 10^9`
- `1 <= len(nums) <= 10^5`
- All numbers in `nums` are positive

## Hints

<details>
<summary>Hint 1</summary>

Once a window's sum reaches the target, making the window *longer* can only make it worse. What should you do with it instead?

</details>

<details>
<summary>Hint 2</summary>

All numbers are positive, so the sum rises as the window grows and falls as it shrinks. That monotonicity is what lets you shrink greedily without missing a shorter answer.

</details>

## Solution

### Intuition

This is the mirror image of a longest-window problem. Instead of growing while the window stays valid, grow until it *becomes* valid — then shrink from the left as far as possible while it remains valid, recording the length at every step.

Because every number is positive, the sum increases monotonically as the window grows and decreases monotonically as it shrinks. That is what makes the greedy shrink safe: the moment the sum drops below `target`, no further shrinking could ever recover it, so there is nothing shorter to find from this right boundary.

```plaintext
nums = [2, 1, 5, 2, 3, 2],  target = 7

end=0   [2]                 sum=2               < 7, grow
end=1   [2, 1]              sum=3               < 7, grow
end=2   [2, 1, 5]           sum=8   >= 7  ->  length 3, drop 2
        [1, 5]              sum=6               < 7, grow
end=3   [1, 5, 2]           sum=8   >= 7  ->  length 3, drop 1
        [5, 2]              sum=7   >= 7  ->  length 2, drop 5   <- best
        [2]                 sum=2               < 7, grow
end=4   [2, 3]              sum=5               < 7, grow
end=5   [2, 3, 2]           sum=7   >= 7  ->  length 3

answer = 2
```

### Algorithm

1. Add `nums[window_end]` to `window_sum`
2. While `window_sum >= target`, record the window length, then subtract `nums[window_start]` and advance `window_start`
3. Continue until `window_end` reaches the end of the array
4. If no valid window was ever found, return `0`

Note that the length is recorded before the removal. In a shortest-window problem the current window is the candidate; in a longest-window problem you record after restoring validity instead.

### Complexity analysis

- Time complexity: $O(n)$ — `window_start` never moves backwards, so across the whole scan each index is added once and removed at most once. The inner `while` runs at most $n$ times in total, not once per outer iteration.
- Space complexity: $O(1)$ — only the running sum, the boundaries, and the best length are stored.

```python
from typing import List


class Solution:
    def min_sub_array_len(self, target: int, nums: List[int]) -> int:
        minimum_length = float("inf")

        window_start = 0
        window_sum = 0
        for window_end in range(len(nums)):
            window_sum += nums[window_end]

            # shrink while the window is still valid, to find the shortest one
            while window_sum >= target:
                minimum_length = min(minimum_length, window_end - window_start + 1)
                window_sum -= nums[window_start]
                window_start += 1

        return 0 if minimum_length == float("inf") else minimum_length
```

The positivity constraint is not decoration. With negative numbers allowed, growing the window no longer increases the sum, so a window failing the test says nothing about longer windows — the sliding window breaks down and the problem needs a prefix sum with a hash map instead.
