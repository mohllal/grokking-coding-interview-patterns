# Problem: Squaring a Sorted Array

LeetCode problem: [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/).

Given a sorted array, create a new array containing squares of all the number of the input array in the sorted order.

## Examples

Example 1:

```plaintext
Input: [-2, -1, 0, 2, 3]
Output: [0, 1, 4, 4, 9]
```

Example 2:

```plaintext
Input: [-3, -1, 0, 1, 2]
Output: [0 1 1 4 9]
```

## Solution 1

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

```python
def sortedSquares(nums: List[int]) -> List[int]:
    squares = [num ** 2 for num in nums]
    return sorted(squares)
```

## Solution 2

First, we identify the absolute minimum element in the input list and its index. This ensures that we start processing from the element with the smallest absolute value in the result list.

Starting from that minimum element's index, we use two pointers to move left or right, appending the smaller square of the two elements at the pointers to the result list and move to the left/right number according to the pointer.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(N)

```python
def sortedSquares(nums: List[int]) -> List[int]:
    squares = []
        
    minimum_element = abs(nums[0])
    minimum_element_index = 0
    for i in range(len(nums)):
        if abs(nums[i]) < minimum_element:
            minimum_element = abs(nums[i])
            minimum_element_index = i

    left = minimum_element_index
    right = minimum_element_index + 1
    
    while left >= 0 and right <= len(nums) - 1:
        if abs(nums[left]) <= abs(nums[right]):
            squares.append(nums[left] ** 2)
            left -= 1
        else:
            squares.append(nums[right] ** 2)
            right += 1

    while left >= 0:
        squares.append(nums[left] ** 2)
        left -= 1
    
    while right <= len(nums) - 1:
        squares.append(nums[right] ** 2)
        right += 1
    
    return squares
```

## Solution 3

Since the numbers at both the ends can give us the largest square value, we will use two pointers at both ends:

At any step, whichever pointer gives us the bigger square value we add it to the result array and move to the left/right number according to the pointer and at the end, we reverse the result list to correct the order from smaller to larger.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(N)

```python
def sortedSquares(nums: List[int]) -> List[int]:
    squares = []
        
    left = 0
    right = len(nums) - 1

    while left <= right:
        leftSquare = nums[left] ** 2
        rightSquare = nums[right] ** 2
        
        if leftSquare >= rightSquare:
            squares.append(leftSquare)
            left += 1
        else:
            squares.append(rightSquare)
            right -= 1
        
    return reversed(squares)
```
