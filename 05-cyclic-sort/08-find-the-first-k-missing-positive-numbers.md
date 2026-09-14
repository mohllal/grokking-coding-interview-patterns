---
title: Find the First K Missing Positive Numbers
difficulty: 🔴 Hard
tags:
  - Array
  - Cyclic Sort
  - Hash Set
---

# Find the First K Missing Positive Numbers

## Problem Description

Given an unsorted array `nums` (which can contain duplicates and negative numbers) and an integer `k`, return the first `k` missing positive numbers.

## Examples

**Example 1:**

```plaintext
Input: nums = [3, -1, 4, 5, 5], k = 3
Output: [1, 2, 6]
Explanation: The smallest missing positive numbers are 1, 2 and 6.
```

**Example 2:**

```plaintext
Input: nums = [2, 3, 4], k = 3
Output: [1, 5, 6]
Explanation: The smallest missing positive numbers are 1, 5 and 6.
```

**Example 3:**

```plaintext
Input: nums = [-2, -3, 4], k = 2
Output: [1, 2]
Explanation: The smallest missing positive numbers are 1 and 2.
```

## Constraints

- `n == len(nums)`
- `1 <= k`
- `nums` may contain duplicates and negative values

## Solution

### Intuition

After cyclic-sorting values `1..n` into their home indices, every index `i` where `nums[i] != i + 1` indicates that `i + 1` is missing.

That gives us missing numbers within `1..n`. If we still need more, the remaining missing positives must be `> n`. To avoid returning a number that actually exists in the array (e.g. `n + 1` might be present), we track the “extra” values we saw after placement using a set.

### Algorithm

1. Run cyclic sort placing any value `x` in `1..n` to index `x - 1`
2. Scan indices from `0..n-1`:
   - If `nums[i] != i + 1`, add `i + 1` to the answer
   - Add `nums[i]` to an `extras` set (values that are out of place / duplicates)
   - Stop if we already have `k` missing numbers
3. If we still need more, append numbers starting from `n + 1` upward, skipping any value present in `extras`, until we have `k`

### Complexity Analysis

- **Time Complexity:** $O(n + k)$ — $O(n)$ for placement and scan, plus up to $O(k)$ to extend beyond `n`.
- **Space Complexity:** $O(n)$ — for the `extras` set (used to avoid returning existing values), plus the output list.

```python
from typing import List


class Solution:
    def firstKMissingPositive(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        i = 0

        while i < n:
          # Value x belongs at index x-1
          correct_idx = nums[i] - 1

          # Rearrange the elements to place each positive integer at its correct index while
          # skipping negative numbers and numbers greater than the array size
          if nums[i] > 0 and nums[i] <= n and nums[i] != nums[correct_idx]:
            nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
          else:
            i += 1

        missing = []
        extras = set()

        for i, num in enumerate(nums):
            # Any index i not holding i+1 means i+1 is missing
            if num != i + 1:
                missing.append(i + 1)
                extras.add(num) # Track values that exist so we can skip them later

                # If we have found k missing numbers, return the result
                if len(missing) == k:
                    return missing

        candidate = n + 1
        while len(missing) < k:
            # Remaining missing positives are > n; skip ones that already exist in the array
            if candidate not in extras:
                missing.append(candidate)

            candidate += 1

        return missing
```
