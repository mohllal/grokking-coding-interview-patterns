# Problem: Remove Duplicates

LeetCode problem: [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/).

Given an array of sorted numbers, remove all duplicates from it. You should not use any extra space; after removing the duplicates in-place return the new length of the array.

## Examples

Example 1:

```plaintext
Input: [2, 3, 3, 3, 6, 9, 9]
Output: 4
Explanation: The first four elements after removing the duplicates will be [2, 3, 6, 9].
```

Example 2:

```plaintext
Input: [2, 2, 2, 11]
Output: 2
Explanation: The first two elements after removing the duplicates will be [2, 11].
```

## Solution

Using two pointers that start at the beginning of the array:

1. The `current` pointer for iterating through the array.
2. The `last_non_duplicate` pointer for keeping track of the position where the next unique element should be placed.

As `current` iterates through the array, if it encounters a new unique value (i.e., one that differs from the value at the `last_non_duplicate` pointer), it increments `last_non_duplicate` and swaps the elements at the `last_non_duplicate` and `current` positions, effectively moving the unique value into its correct position.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def removeDuplicates(nums: List[int]) -> int:
    current = 0
    last_non_duplicate = 0
    
    while current < len(nums):
        if nums[last_non_duplicate] != nums[current]:
            last_non_duplicate += 1
            nums[last_non_duplicate], nums[current] = nums[current], nums[last_non_duplicate]
            
        current += 1
        
    return last_non_duplicate + 1
```

## Similar Problem

LeetCode problem: [27. Remove Element](https://leetcode.com/problems/remove-element/).

Given an unsorted array of numbers and a target `key`, remove all instances of `key` in-place and return the new length of the array.

### Examples

Example 1:

```plaintext
Input: [3, 2, 3, 6, 3, 10, 9, 3], key = 3
Output: 4
Explanation: The first four elements after removing every 'key' will be [2, 6, 10, 9].
```

Example 2:

```plaintext
Input: [2, 11, 2, 2, 1], key = 2
Output: 2
Explanation: The first two elements after removing every 'key' will be [11, 1].
```

### Solution

This algorithm removes all instances of a given value val from a list nums in-place.

It uses two pointers:

1. The `current` pointer for iterating through the array.
2. The `next_non_val` pointer for keeping track of the position where the next non-`val` element should be placed.

When `current` finds an element not equal to `val`, it swaps it with the element at `next_non_val`, ensuring all non-`val` elements are moved to the front of the array.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def removeElement(nums: List[int], val: int) -> int:
    current = 0
    next_non_val = 0

    while current < len(nums):
        if nums[current] != val:
            nums[next_non_val], nums[current] = nums[current], nums[next_non_val]
            next_non_val += 1

        current += 1

    return next_non_val
```
