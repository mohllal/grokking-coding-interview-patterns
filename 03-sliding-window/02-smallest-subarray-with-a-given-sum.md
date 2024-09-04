# Problem: Smallest Subarray with a Given Sum

LeetCode problem: [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/).

Given an array of positive numbers and a positive number `S`, find the length of the smallest contiguous subarray whose sum is greater than or equal to `S`. Return `0`, if no such subarray exists.

## Examples

Example 1:

```plaintext
Input: [2, 1, 5, 2, 3, 2], S = 7 
Output: 2
Explanation: The smallest subarray with a sum great than or equal to '7' is [5, 2].
```

Example 2:

```plaintext
Input: [2, 1, 5, 2, 8], S = 7 
Output: 1
Explanation: The smallest subarray with a sum greater than or equal to '7' is [8].
```

Example 3:

```plaintext
Input: [3, 4, 1, 1, 6], S = 8 
Output: 3
Explanation: Smallest subarrays with a sum greater than or equal to '8' are [3, 4, 1] or [1, 1, 6].
```

## Solution

The algorithm implements a dynamic sliding window approach that adjusts its size based on the sum of the elements within the window.

1. Starting from the beginning of the array, it adds elements to the window until their sum meets or exceeds the target `S`.
2. Once the window's sum meets or exceeds the target `S`, the algorithm attempts to minimize the window size by shrinking it from the start.
3. While shrinking, two actions occur:
    1. It checks if the current window length is the smallest found so far and stores its length if so.
    2. It reduces the sum by removing the first element of the window and continues shrinking until the sum falls below the target `S`.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

Why time complexity is O(N)?

The algorithm uses a sliding window approach where each element is processed at most twice: once when it is added to the window and once when it is removed from the window (while shrinking the window).

This gives a linear time complexity of O(N), where `N` is the number of elements in the input array.

```python
def minSubArrayLen(target: int, nums: List[int]) -> int:
    minimum_length = float("inf")
    
    window_start = 0
    window_sum = 0
    for window_end in range(len(nums)):    
        window_sum += nums[window_end]
        
        while window_sum >= target:
            minimum_length = min(minimum_length, (window_end - window_start + 1))
            window_sum -= nums[window_start]
            window_start += 1
    
    return 0 if minimum_length == float("inf") else minimum_length
```
