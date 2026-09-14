---
title: Longest Subarray with Ones after Replacement
difficulty: Medium
leetcode_title: Max Consecutive Ones III
leetcode: https://leetcode.com/problems/max-consecutive-ones-iii/
tags:
  - Array
  - Binary Search
  - Sliding Window
  - Prefix Sum
---

# Longest Subarray with Ones after Replacement

## Problem description

Given an array containing `0`s and `1`s, if you are allowed to replace no more than `k` `0`s with `1`s, find the length of the longest contiguous subarray having all `1`s.

## Examples

**Example 1:**

```plaintext
Input: nums = [0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1], k = 2
Output: 6
Explanation: Replace the '0' at index 5 and 8 to have the longest contiguous subarray of 1s having length 6.
```

**Example 2:**

```plaintext
Input: nums = [0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], k = 3
Output: 9
Explanation: Replace the '0' at index 6, 9, and 10 to have the longest contiguous subarray of 1s having length 9.
```

## Constraints

- `1 <= len(nums) <= 10^5`
- `nums[i]` is either `0` or `1`
- `0 <= k <= len(nums)`

## Hints

<details>
<summary>Hint 1</summary>

Rather than asking which zeros to flip, ask what makes a window affordable. A window is usable when it contains at most `k` zeros.

</details>

<details>
<summary>Hint 2</summary>

You do not need to count zeros directly. If you know the window's length and how many ones it holds, the number of zeros follows.

</details>

## Solution

### Intuition

With only two possible values, "the most frequent element" is no longer a question — the letter you keep is always `1`. So the cost formula collapses to counting zeros:

```plaintext
zeros in window = window_length - ones_in_window
```

The window is valid while that count is at most `k`. Grow on every step and shrink from the left whenever there are too many zeros to flip.

```plaintext
nums = [0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1],  k = 2

end= 0  [0]                   len 1, ones 0, zeros 1   valid    length 1
end= 1  [0,1]                 len 2, ones 1, zeros 1   valid    length 2
end= 2  [0,1,1]               len 3, ones 2, zeros 1   valid    length 3
end= 3  [0,1,1,0]             len 4, ones 2, zeros 2   valid    length 4
end= 4  [0,1,1,0,0]           len 5, ones 2, zeros 3   3 > 2, shrink
        [1,1,0,0]             len 4, ones 2, zeros 2   valid    length 4
end= 5  [1,1,0,0,0]           len 5, ones 2, zeros 3   3 > 2, shrink twice
        [0,0]                 len 2, ones 0, zeros 2   valid    length 2
end= 6  [0,0,1]               len 3, ones 1, zeros 2   valid    length 3
end= 7  [0,0,1,1]             len 4, ones 2, zeros 2   valid    length 4
end= 8  [0,0,1,1,0]           len 5, ones 2, zeros 3   3 > 2, shrink
        [0,1,1,0]             len 4, ones 2, zeros 2   valid    length 4
end= 9  [0,1,1,0,1]           len 5, ones 3, zeros 2   valid    length 5
end=10  [0,1,1,0,1,1]         len 6, ones 4, zeros 2   valid    length 6   <- best

answer = 6
```

### Algorithm

1. If `nums[window_end]` is `1`, increment `window_ones_count`
2. While `window_length - window_ones_count > k`, the window holds more zeros than can be flipped: decrement the count if `nums[window_start]` is `1`, then advance `window_start`
3. Record `window_end - window_start + 1` as a candidate for the longest length

### Complexity analysis

- Time complexity: $O(n)$ — each index enters and leaves the window at most once.
- Space complexity: $O(1)$ — a single counter replaces the frequency map, since there are only two possible values.

```python
from typing import List


class Solution:
    def longest_ones(self, nums: List[int], k: int) -> int:
        longest_length = 0

        window_start = 0
        window_ones_count = 0
        for window_end in range(len(nums)):
            window_ones_count += nums[window_end]

            # zeros in the window = length - ones; shrink while too many to flip
            while (window_end - window_start + 1) - window_ones_count > k:
                window_ones_count -= nums[window_start]
                window_start += 1

            longest_length = max(longest_length, window_end - window_start + 1)

        return longest_length
```

Because the values are exactly `0` and `1`, adding `nums[window_end]` directly is the same as testing for `1` and incrementing — the same trick that makes the subtraction on shrink unconditional.

## Relationship to [Longest Substring with Same Letters after Replacement](./06-longest-substring-with-same-letters-after-replacement.md)

This is that problem restricted to a two-symbol alphabet. There, the cost of a window is `length - count_of_most_frequent_letter`, and finding that most frequent letter needs a frequency map.

Here, `1` is always the letter worth keeping, so the map collapses to a single integer and the awkward `max_letter_count` bookkeeping disappears entirely — along with the subtle overestimation argument that problem needs.
