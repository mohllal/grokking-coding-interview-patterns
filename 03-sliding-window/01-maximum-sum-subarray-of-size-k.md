# Problem: Maximum Sum Subarray of Size K

LeetCode problem: [1708. Largest Subarray Length K](https://leetcode.com/problems/largest-subarray-length-k/).

Given an array of positive numbers and a positive number `k`, find the maximum sum of any contiguous subarray of size `k`.

## Examples

Example 1:

```plaintext
Input: [2, 1, 5, 1, 3, 2], k = 3 
Output: 9
Explanation: Subarray with maximum sum is [5, 1, 3].
```

Example 2:

```plaintext
Input: [2, 3, 4, 1, 5], k = 2 
Output: 7
Explanation: Subarray with maximum sum is [3, 4].
```

## Solution 1

This algorithm uses a brute approach that calculates the sum of all `k` sized subarrays of the input array to find the subarray with the maximum sum.

It iterates through each starting index of the array and calculates the sum of the next `k` contiguous elements, updating the maximum sum encountered along the way.

Complexity analysis:

- Time complexity: O(N * K)
- Space complexity: O(1)

```python
def findMaxSum(nums: List[int], k: int) -> int:
    maximum_sum = 0

    for i in range(len(nums) - k + 1):
        window_sum = 0
        for j in range(i, i + k):
            window_sum += nums[j]
        
        maximum_sum = max(maximum_sum, window_sum)

    return maximum_sum
```

## Solution 2

This algorithm uses a sliding window of size `k` to compute the sum of subarrays as it moves across the array. Instead of recalculating the sum for every subarray from scratch, it updates the sum by adjusting for the sliding window's movement.

Steps:

1. Subtract the element that is exiting the sliding window (i.e., the first element of the window).
2. Add the new element that is entering the sliding window (i.e., the element immediately following the current window).

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def findMaxSum(nums: List[int], k: int) -> int:
    maximum_sum = 0
    
    window_start = 0
    window_sum = 0
    for window_end in range(len(nums)):
        window_sum += nums[window_end]
        
        if window_end >= k - 1:
            maximum_sum = max(maximum_sum, window_sum)
            window_sum -= nums[window_start]
            window_start += 1
    
    return maximum_sum
```

## Similar Problem

LeetCode problem: [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/).

Given an array, find the maximum average of any contiguous subarray of size `k`.

### Examples

Example 1:

```plaintext
Input: nums = [1, 12, -5, -6, 50, 3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
```

Example 2:

```plaintext
Input: nums = [5], k = 1
Output: 5.00000
```

### Solution

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def findMaxAverage(nums: List[int], k: int) -> float:
    maximum_average = 0
    
    window_start = 0
    window_sum = 0
    for window_end in range(len(nums)):
        window_sum += nums[window_end]
        
        if window_end >= k - 1:
            maximum_average = max(maximum_average, round(window_sum / k, 5))
            window_sum -= nums[window_start]
            window_start += 1
    
    return maximum_average
```
