# Problem: Pair with Target Sum

LeetCode problem: [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/).

Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

Write a function to return the indices of the two numbers (i.e. the pair) such that they add up to the given target.

## Examples

Example 1:

```plaintext
Input: [1, 2, 3, 4, 6], target = 6
Output: [1, 3]
Explanation: The numbers at index 1 and 3 add up to 6: 2 + 4 = 6
```

Example 2:

```plaintext
Input: [2, 5, 9, 11], target = 11
Output: [0, 2]
Explanation: The numbers at index 0 and 2 add up to 11: 2 + 9 = 11
```

## Solution 1

For each element in the list, a binary search is performed to find if the complement of that element (i.e., `target - numbers[i]`) exists in the list.

If a match is found, it returns the indices of the two numbers (adjusted to be 1-based); otherwise, it returns `-1` if no such pair is found.

Complexity analysis:

- Time complexity: O(N * logN)
- Space complexity: O(1)

```python
def binary_search(numbers: List[int], target: int):
    low = 0
    high = len(numbers) - 1

    while low <= high:
        mid = (low + high) // 2

        if numbers[mid] > target:
            high = mid - 1
        elif numbers[mid] < target:
            low = mid + 1
        else:
            return mid

    return -1

def twoSum(numbers: List[int], target: int) -> List[int]:
    for i in range(len(numbers)):
        j = binary_search(numbers, target - numbers[i])
        if j != -1:
            return [i + 1, j + 1]

    return -1
```

## Solution 2

Using two pointers at the ends of the array:

1. If the sum of the elements at both pointers equals the target, return the result.
2. If the sum is greater than the target, reduce the sum by moving the right pointer leftward (since the array is sorted, this brings in a smaller element).
3. If the sum is less than the target, increase the sum by moving the left pointer rightward (this brings in a larger element, as the array is sorted).

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def twoSum(numbers: List[int], target: int) -> List[int]:
    i = 0
    j = len(numbers) - 1

    while i < j:
        if numbers[i] + numbers[j] > target:
            j -= 1
        elif numbers[i] + numbers[j] < target:
            i += 1
        else:
            return [i + 1, j + 1]

    return -1
```
