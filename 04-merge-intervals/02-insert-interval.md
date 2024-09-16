# Problem: Insert Interval

LeetCode problem: [57. Insert Interval](https://leetcode.com/problems/insert-interval/).

Given a list of non-overlapping intervals sorted by their start time, insert a given interval at the correct position and merge all necessary intervals to produce a list that has only mutually exclusive intervals.

## Examples

Example 1:

```plaintext
Input: Intervals = [[1,3], [5,7], [8,12]], New Interval = [4,6]
Output: [[1,3], [4,7], [8,12]]
Explanation: After insertion, since [4,6] overlaps with [5,7], we merged them into one [4,7].
```

Example 2:

```plaintext
Input: Intervals = [[1,3], [5,7], [8,12]], New Interval = [4,10]
Output: [[1,3], [4,12]]
Explanation: After insertion, since [4,10] overlaps with [5,7] & [8,12], we merged them into [4,12].
```

Example 3:

```plaintext
Input: Intervals = [[2,3],[5,7]], New Interval = [1,4]
Output: [[1,4], [5,7]]
Explanation: After insertion, since [1,4] overlaps with [2,3], we merged them into one [1,4].
```

## Solution

The algorithm follows these steps:

1. Start by adding intervals that end before the new interval begins, as they don’t overlap.
2. For intervals that overlap with the new interval, adjust the boundaries of the new interval by expanding it to cover the entire range of the overlaps.
3. Add the newly merged interval to the result.
4. Append any intervals that start after the new interval, which now represent the all merged intervals, as they do not overlap and can be added as-is.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(N)

```python
def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
    merged = []
    
    # skip non-overlapping intervals that end before the new interval start
    i = 0
    while i < len(intervals) and intervals[i][1] < newInterval[0]:
        merged.append(intervals[i])
        i += 1
    
    # merge overlapping intervals
    while i < len(intervals) and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(intervals[i][0], newInterval[0])
        newInterval[1] = max(intervals[i][1], newInterval[1])
        i += 1
    
    merged.append(newInterval)
    
    # skip non-overlapping intervals after merging overlapping ones
    while i < len(intervals):
        merged.append(intervals[i])
        i += 1
    
    return merged
```
