# Problem: Minimum Window Sort

LeetCode problem: [581. Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/).

Given an array, find the length of the smallest subarray in it which when sorted will sort the whole array.

## Examples

Example 1:

```plaintext
Input: [1, 2, 5, 3, 7, 10, 9, 12]
Output: 5
Explanation: We need to sort only the subarray [5, 3, 7, 10, 9] to make the whole array sorted
```

Example 2:

```plaintext
Input: [1, 3, 2, 0, -1, 7, 10]
Output: 5
Explanation: We need to sort only the subarray [1, 3, 2, 0, -1] to make the whole array sorted
```

Example 3:

```plaintext
Input: [1, 2, 3]
Output: 0
Explanation: The array is already sorted
```

Example 4:

```plaintext
Input: [3, 2, 1]
Output: 3
Explanation: The whole array needs to be sorted.
```

## Solution 1

The algorithm first creates a sorted copy of the input array. It then iterates through both the input and sorted arrays to identify the start and end indices of the unsorted subarray.

By comparing corresponding elements in the input array and the sorted array, it finds the first and last positions where the elements differ.

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

```python
def findUnsortedSubarray(nums: List[int]) -> int:
    sorted_nums = list(sorted(nums))
    
    start = None
    end = None
    for i in range(len(nums)):
        if nums[i] != sorted_nums[i]:
            if start is None:
                start = i
                end = i
            else:
                end = i

    return (end - start) + 1 if start is not None else 0
```

## Solution 2

The algorithm starts by identifying both the start and the end of the candidate unsorted subarray through the following steps:

1. It begins from the start of the array to find the first element that is out of order (i.e., an element smaller than its previous element).
2. Similarly, it starts from the end of the array to find the first element that is out of order (i.e., an element larger than its previous element).

But sorting this subarray may not necessarily result in the entire array being sorted. Here is an example:

```plaintext
Input: [1, 3, 2, 0, -1, 7, 10]
Sorting the numbers between '3' and '-1' will not sort the whole array: [1, -1, 0, 2, 3, 7, 10]
```

This happens when any of the following two conditions is met:

1. There is a number before the candidate unsorted subarray that is larger than any number in the unsorted subarray.
2. There is a number after the candidate unsorted subarray that is smaller than any number in the unsorted subarray.

To sort the entire array, it is necessary to include all such elements that are larger than the smallest element of the unsorted subarray and all elements that are smaller than the largest element of the unsorted subarray.

Therefore, the final algorithm proceeds as follows:

1. From the beginning of the array, find the first element that is out of order. This element marks the start of the candidate unsorted subarray.
2. From the end of the array, find the first element that is out of order. This element marks the end of the candidate unsorted subarray.
3. Find the minimum and maximum values within this unsorted subarray.
4. Extend the subarray to the left to include any elements that are larger than the minimum value of the unsorted subarray.
5. Similarly, extend the subarray to the right to include any elements that are smaller than the maximum value of the unsorted subarray.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
# O(N) time and O(1) space
def findUnsortedSubarrayIndices(nums: List[int]) -> Tuple[Optional[int], Optional[int]]:
    left = 0
    while left < len(nums) - 1 and nums[left] <= nums[left + 1]:
        left += 1
    
    # if the array is sorted
    if left == len(nums) - 1:
        return None, None
    
    right = len(nums) - 1
    while right > 0 and nums[right] >= nums[right - 1]:
        right -= 1
    
    return left, right

# O(N) time and O(1) space
def findSubarrayMinimumAndMaximum(nums: List[int], start: int, end: int) -> Tuple[int, int]:
    minimum = float("inf")
    maximum = float("-inf")
    for i in range(start, end + 1):
        minimum = min(minimum, nums[i])
        maximum = max(maximum, nums[i])
    
    return minimum, maximum

def findUnsortedSubarray(nums: List[int]) -> int:
    start, end = findUnsortedSubarrayIndices(nums)
    
    # if the array is sorted
    if start is None and end is None:
        return 0

    # find minimum and maximum numbers in the unsorted subarray
    minimum, maximum = findSubarrayMinimumAndMaximum(nums, start, end)
    
    # expand the subarray to the left to include any element before the subarray
    # which is smaller than the minimum element of the unsorted subarray
    temp = start - 1
    while temp >= 0:
        if nums[temp] > minimum:
            start = temp
        temp -= 1
    
    # expand the subarray to the right to include any element after the subarray
    # which is larger than the maximum element of the unsorted subarray
    temp = end + 1
    while temp <= len(nums) - 1:
        if nums[temp] < maximum:
            end = temp
        temp += 1
        
    return (end - start) + 1
```
