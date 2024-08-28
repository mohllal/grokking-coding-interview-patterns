# Problem: Triplets with Smaller Sum

LeetCode problem: [3Sum Smaller](https://leetcode.com/problems/3sum-smaller/).

Given an array arr of unsorted numbers and a target sum, count all triplets in it such that `arr[i] + arr[j] + arr[k] < target` where `i`, `j`, and `k` are three different indices.

Write a function to return the count of such triplets.

## Examples

Example 1:

```plaintext
Input: [-1, 0, 2, 3], target = 3 
Output: 2
Explanation: There are two triplets whose sum is less than the target: [-1, 0, 3], [-1, 0, 2]
```

Example 2:

```plaintext
Input: [-1, 4, 2, 1, 3], target = 5 
Output: 4
Explanation: There are four triplets whose sum is less than the target: [-1, 1, 4], [-1, 1, 3], [-1, 1, 2], [-1, 2, 3]
```

## Solution

This problem is quite similar to the [Triplet Sum to Zero](./04-triplet-sum-to-zero.md) problem.

First, we sort the array. Then, for each element `X`, we use two pointers: one starting from the element after it `Y`, and the other from the end of the list `Z`.

We calculate the sum of the triplet formed by these three elements and evaluate these two cases:

1. If the sum of `X + Y + Z` is greater than or equal to the target, decrement the right pointer (to reduce the sum).
2. Otherwise, increment the triplets count by `(right - left)` because all pairs between `Y` and `Z` alongside the current element `X` will form valid triplets (because these values must be less than or equal to `Z`). Then, move the `left` pointer to try the next element as `Y` and explore further triplets.

Complexity analysis:

- Time complexity: O(N^2)
- Space complexity: O(1)

```python
def threeSumSmaller(nums: List[int], target: int) -> int:
    triplets = 0
    nums.sort()

    for i in range(len(nums)):
        if nums[i] >= target:
            break
    
        left = i + 1
        right = len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]

            if total >= target:
                right -= 1
            else:
                # since arr[right] >= arr[left], therefore, we can replace arr[right]
                # by any number between left and right to get a sum less than the target
                triplets += (right - left)
                left += 1
    
    return triplets
```

## Similar Problem

Write a function to return the list of all such triplets instead of the count. How will the time complexity change in this case?

### Solution

Complexity analysis:

- Time complexity: O(N^3)
- Space complexity: O(N^3)

Why space complexity is O(N^3)?

The equation for the number of combinations of `K` elements from an array of size `N` (also known as "`N` choose `K`") is given by the binomial coefficient formula:

```math
\binom{N}{K} = \frac{N!}{K!(N - K)!}
```

For $K = 3$:

```math
\binom{N}{3} = \frac{N!}{3!(N - 3)!} = \frac{N \times (N - 1) \times (N - 2) \times (N - 3)!}{3! \times (N - 3)!}
```

Canceling the $(N - 3)!$ terms:

```math
\binom{N}{3} = \frac{N \times (N - 1) \times (N - 2)}{3!} = \frac{N^3 - 3N^2 + 2N}{6}
```

Since the result array will hold maximum of $3 \times \binom{N}{3}$ elements in case that all triplets in the array are valid triplets, thus the space complexity upper bound will be:

```math
O({3 \times (\frac{N^3 - 3N^2 + 2N}{6})}) = O({N^3 - 3N^2 + 2N}) = O(N^3)
```

```python
def threeSumSmaller(nums: List[int], target: int) -> int:
    triplets = []
    nums.sort()

    for i in range(len(nums)):
        if nums[i] >= target:
            break
    
        left = i + 1
        right = len(nums) - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]

            if total >= target:
                right -= 1
            else:
                # since arr[right] >= arr[left], therefore, we can replace arr[right]
                # by any number between left and right to get a sum less than the target
                temp = right
                while temp > left:
                    triplets.append([nums[i], nums[left], nums[temp]])
                    temp -= 1

                left += 1
    
    return triplets
```
