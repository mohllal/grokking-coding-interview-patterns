# Problem: Longest Subarray with Ones after Replacement

LeetCode problem: [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/).

Given an array containing `0`s and `1`s, if you are allowed to replace no more than `K` `0`s with `1`s, find the length of the longest contiguous subarray having all `1`s.

## Examples

Example 1:

```plaintext
Input: Array = [0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1], k = 2
Output: 6
Explanation: Replace the '0' at index 5 and 8 to have the longest contiguous subarray of 1s having length 6.
```

Example 2:

```plaintext
Input: Array = [0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], k = 3
Output: 9
Explanation: Replace the '0' at index 6, 9, and 10 to have the longest contiguous subarray of 1s having length 9.
```

## Solution

This problem is quite similar to the [Longest Substring with Same Letters after Replacement](./06-longest-substring-with-same-letters-after-replacement.md) problem with the key difference being that the input consists only of binary values (0s and 1s).

The algorithm uses a sliding window approach to iterate through the binary array. It tracks the count of `1`s within the current window (referred to as `window_ones_count`). The goal is to maintain a valid window where the number of `0`s in the window, calculated using `(window size) - (window ones count)`, is less than or equal to `K`.

If the number of `0`s exceeds `K`, we shrink the window from the left to maintain this condition.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def longestOnes(nums: List[int], k: int) -> int:
    longest_length = 0
    
    window_start = 0
    window_ones_count = 0
    for window_end in range(len(nums)):
        window_end_elem = nums[window_end]
        if window_end_elem == 1:
            window_ones_count += 1

        while (window_end - window_start + 1) - window_ones_count > k:
            window_start_elem = nums[window_start]
            if window_start_elem == 1:
                window_ones_count -= 1

            window_start += 1
        
        longest_length = max(longest_length, (window_end - window_start) + 1)
    
    return longest_length
```
