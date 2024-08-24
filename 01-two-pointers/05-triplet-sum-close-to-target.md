# Problem: Triplet Sum Close to Target

LeetCode problem: [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/).

Given an array of unsorted numbers and a target number, find a triplet in the array whose sum is as close to the target number as possible, return the sum of the triplet.

If there are more than one such triplet, return the sum of the triplet with the smallest sum.

## Examples

Example 1:

```plaintext
Input: [-2, 0, 1, 2], target = 2
Output: 1
Explanation: The triplet [-2, 1, 2] has the closest sum to the target.
```

Example 2:

```plaintext
Input: [-3, -1, 1, 2], target = 1
Output: 0
Explanation: The triplet [-3, 1, 2] has the closest sum to the target.
```

Example 3:

```plaintext
Input: [1, 0, 1, 1], target = 100
Output: 3
Explanation: The triplet [1, 1, 1] has the closest sum to the target.
```

## Solution

This problem is quite similar to the [Triplet Sum to Zero](./04-triplet-sum-to-zero.md) problem.

First, we sort the array. Then, for each element, we use two pointers: one starting from the element after the current one, and the other from the end of the list. We calculate the difference between the sum of the triplet formed by these three elements and the target value while tracking the closest sum.

Complexity analysis:

- Time complexity: O(N^2)
- Space complexity: O(1)

```python
def threeSumClosest(nums: List[int], target: int) -> int:
    nums.sort()
    closest = float('inf')
    
    for i in range(len(nums)):
        j = i + 1
        k = len(nums) - 1

        while j < k:
            diff = target - (nums[i] + nums[j] + nums[k])
            
            if abs(diff) < abs(closest):
                closest = diff
            
            if diff > 0:
                j += 1 # we need a triplet with bigger sum
            elif diff < 0:
                k -= 1 # we need a triplet with smaller sum
            else:
                return target - closest

    return target - closest
```
