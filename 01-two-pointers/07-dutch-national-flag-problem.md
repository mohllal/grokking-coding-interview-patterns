# Problem: Dutch National Flag Problem

LeetCode problem: [75. Sort Colors](https://leetcode.com/problems/sort-colors/).

Given an array containing 0s, 1s and 2s, sort the array in-place. You should treat numbers of the array as objects, hence, we can’t count 0s, 1s, and 2s to recreate the array.

The flag of the Netherlands consists of three colors: red, white and blue; and since our input array also consists of three different numbers that is why it is called [Dutch National Flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem).

## Examples

Example 1:

```plaintext
Input: [1, 0, 2, 1, 0]
Output: [0 0 1 1 2]
```

Example 2:

```plaintext
Input: [2, 2, 0, 1, 2, 0]
Output: [0 0 1 2 2 2 ]
```

## Solution 1

The algorithm sorts an array containing `0`s (red), `1`s (white), and `2`s (blue) by making two passes. The first pass moves all `0`s to the front, and the second pass moves all `2`s to the end. The `1`s naturally fall into place between the `0`s and `2`s.

Complexity analysis:

- Time complexity: O(N) - two-pass algorithm
- Space complexity: O(1)

```python
def sortColors(nums: List[int]) -> None:
    last_red = 0
    for i in range(len(nums)):
        if nums[i] == 0:
            nums[last_red], nums[i] = nums[i], nums[last_red]
            last_red += 1
            
    last_blue = len(nums) - 1
    for i in range(len(nums) - 1, -1, -1):
        if nums[i] == 2:
            nums[last_blue], nums[i] = nums[i], nums[last_blue]
            last_blue -= 1
  
    return nums
```

## Solution 2

The algorithm uses three pointers:

1. `red` to track the position for `0`s.
2. `blue` to track the position for `2`s.
3. `current` for traversing the array

During iteration, `0`s are swapped to the front (before `red` pointer), `2`s are swapped to the back (after `blue` pointer), and `1`s are left in place.

Complexity analysis:

- Time complexity: O(N) - one-pass algorithm
- Space complexity: O(1)

```python
def sortColors(nums: List[int]) -> None:
    red = 0
    white = 0
    blue = len(nums) - 1
    
    while white <= blue:
        if nums[white] == 0:
            nums[white], nums[red] = nums[red], nums[white]
            white += 1
            red += 1
        elif nums[white] == 2:
            nums[white], nums[blue] = nums[blue], nums[white]
            blue -= 1
        else:
            white += 1
            
    return nums
```
