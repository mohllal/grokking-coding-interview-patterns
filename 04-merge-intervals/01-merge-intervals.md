# Problem: Merge Intervals

LeetCode problem: [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/).

Given a list of intervals, merge all the overlapping intervals to produce a list that has only mutually exclusive intervals.

## Examples

Example 1:

```plaintext
Intervals: [[1,4], [2,5], [7,9]]
Output: [[1,5], [7,9]]
Explanation: Since the first two intervals [1,4] and [2,5] overlap, we merged them into one [1,5].
```

Example 2:

```plaintext
Intervals: [[6,7], [2,4], [5,9]]
Output: [[2,4], [5,9]]
Explanation: Since the intervals [6,7] and [5,9] overlap, we merged them into one [5,9].
```

Example 3:

```plaintext
Intervals: [[1,4], [2,6], [3,5]]
Output: [[1,6]]
Explanation: Since all the given intervals overlap, we merged them into one.
```

## Solution

For two intervals (`a` and `b`) such where `a.start <= b.start`. There are four possible scenarios:

1. `a` and `b` do not overlap.
2. `a` and `b` overlap, `b` ends after `a`.
3. `a` fully overlaps `b`.
4. `b` fully overlaps `a`, but both `a` and `b` have the same start time.

Our goal is to merge the intervals whenever they overlap. For the above-mentioned three overlapping scenarios (2, 3, and 4), this is how we will merge them:

1. `a` and `b` overlap, `b` ends after `a`: interval `c` where `c.start = a.start` and `c.end = b.end`.
2. `a` fully overlaps `b`: interval `c` where `c.start = a.start` and `c.end = a.end`.
3. `b` fully overlaps `a`, but both `a` and `b` have the same start time: interval `c` where `c.start = a.start` and `c.end = b.end`.

So the algorithm follows these steps:

1. Sorts the input array to ensure `a.start <= b.start` for any two intervals `a` and `b`.
2. If `a` overlaps `b` (i.e. `b.start <= a.end`), we need to merge them into a new interval `c` such that: `c.start = a.start` and `c.end = max(a.end, b.end)`.
3. Continue merging interval `c` with the next interval if they overlap. Once no further overlap exists, add the interval `c` to the result array.

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

```python
def merge(intervals: List[List[int]]) -> List[List[int]]:
    merged = []
    intervals.sort(key=lambda interval: interval[0])
    
    interval_start = intervals[0][0]
    interval_end = intervals[0][1]
    for i in range(1, len(intervals)):
        curr_interval_start = intervals[i][0]
        curr_interval_end = intervals[i][1]

        if curr_interval_start <= interval_end:
            interval_end = max(curr_interval_end, interval_end)
        else:
            merged.append([interval_start, interval_end])
            interval_start = curr_interval_start
            interval_end = curr_interval_end
    
    merged.append([interval_start, interval_end])
    return merged
```

## Similar Problem

Given a set of intervals, find out if any two intervals overlap.

### Examples

Example 1:

```plaintext
Intervals: [[1,4], [2,5], [7,9]]
Output: true
Explanation: Intervals [1,4] and [2,5] overlap
```

### Solution

The algorithm follows the same approach as discussed above to find if any two intervals overlap.

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(1)

```python
def isIntersect(intervals: List[List[int]]) -> bool:
    intervals.sort(key=lambda interval: interval[0])
    
    for i in range(1, len(intervals)):
        curr_interval_start = intervals[i][0]
        curr_interval_end = intervals[i][1]

        prev_interval_start = intervals[i - 1][0]
        prev_interval_end = intervals[i - 1][1]        

        if curr_interval_start <= prev_interval_end:
            return True
    
    return False
```
