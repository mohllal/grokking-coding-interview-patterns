# Problem: Quadruple Sum to Target

LeetCode problem: [18. 4Sum](https://leetcode.com/problems/4sum/).

Given an array of unsorted numbers and a target number, find all unique quadruplets in it, whose sum is equal to the target number.

## Examples

Example 1:

```plaintext
Input: [4, 1, 2, -1, 1, -3], target=1
Output: [-3, -1, 1, 4], [-3, 1, 1, 2]
Explanation: Both the quadruplets add up to the target.
```

Example 2:

```plaintext
Input: [2, 0, -1, 1, -2, 2], target=2
Output: [-2, 0, 2, 2], [-1, 0, 1, 2]
Explanation: Both the quadruplets add up to the target.
```

## Solution

This problem is quite similar to the [Triplet Sum to Zero](./04-triplet-sum-to-zero.md) problem.

The algorithm uses two nested loops to select the first two numbers of the quadruplet and a two-pointer approach to search for the remaining two numbers.

Complexity analysis:

- Time complexity: O(N^3)
- Space complexity: O(N^4)

Why space complexity is O(N^4)?
Refer back to the space complexity analysis of the [Triplets with Smaller Sum](./06-triplets-with-smaller-sum.md#Similar-Problem) for more information about this except that it is now the upper bound determined the the binomial coefficient formula $\binom{N}{4} = \frac{N!}{4!(N - 4)!}$.

```python
# O(N) time and O(1) space
def searchPair(nums, i, j, quadruplets):
    left = j + 1
    right = len(nums) - 1

    while left < right:
        total = nums[i] + nums[j] + nums[left] + nums[right]

        if total < target:
            left += 1
        elif total > target:
            right -= 1
        else:
            quadruplets.append([nums[i], nums[j], nums[left], nums[right]])
            left += 1
            right -= 1
            
            while left < right and nums[left] == nums[left - 1]:
                left += 1
                
            while left < right and nums[right] == nums[right + 1]:
                right -= 1    

def fourSum(nums: List[int], target: int) -> List[List[int]]:
    quadruplets = []
    nums.sort()            

    for i in range(len(nums) - 3):
        if i > 0 and nums[i] == nums[i - 1]:
            continue

        for j in range(i + 1, len(nums) - 2):
            if j > i + 1 and nums[j] == nums[j - 1]:
                continue

            searchPair(nums, i, j, quadruplets)
    
    return quadruplets
```
