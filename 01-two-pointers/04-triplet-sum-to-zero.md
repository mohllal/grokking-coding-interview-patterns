# Problem: Triplet Sum to Zero

LeetCode problem: [15. 3Sum](https://leetcode.com/problems/3sum/).

Given an array of unsorted numbers, find all unique triplets in it that add up to zero.

## Examples

Example 1:

```plaintext
Input: [-3, 0, 1, 2, -1, 1, -2]
Output: [-3, 1, 2], [-2, 0, 2], [-2, 1, 1], [-1, 0, 1]
Explanation: There are four unique triplets whose sum is equal to zero.
```

Example 2:

```plaintext
Input: [-5, 2, -1, -2, 3]
Output: [[-5, 2, 3], [-2, -1, 3]]
Explanation: There are two unique triplets whose sum is equal to zero.
```

## Solution

First, we sort the array. Then, for each element `X`, we reduce the problem to finding two elements, `Y` and `Z`, whose sum equals `-X` using a two-pointer approach.

To prevent duplicate triplets, we use a set which slightly impacts the space complexity, that remains `O(N)` due to the need for storing the result list.

Complexity analysis:

- Time complexity: O(N^2)
- Space complexity: O(N)

```python
def threeSum(nums: List[int]) -> List[List[int]]:
    nums.sort()
    triples = set()

    for i in range(len(nums)):
        target = -nums[i]

        j = i + 1
        k = len(nums) - 1
      
        while j < k:
            current = nums[j] + nums[k]
            
            if current < target:
                j += 1
            elif current > target:
                k -= 1
            else:
                triples.add((nums[i], nums[j], nums[k]))
                j += 1
                k -= 1
        
    return triples
```
