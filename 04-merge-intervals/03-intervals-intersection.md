# Problem: Intervals Intersection

LeetCode problem: [986. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/).

Given two lists of intervals, find the intersection of these two lists. Each list consists of disjoint intervals sorted on their start time.

## Examples

Example 1:

```plaintext
Input: arr1=[[1, 3], [5, 6], [7, 9]], arr2=[[2, 3], [5, 7]]
Output: [2, 3], [5, 6], [7, 7]
Explanation: The output list contains the common intervals between the two lists.
```

Example 2:

```plaintext
Input: arr1=[[1, 3], [5, 7], [9, 12]], arr2=[[5, 10]]
Output: [5, 7], [9, 10]
Explanation: The output list contains the common intervals between the two lists.
```

## Solution

The algorithm follows these steps:

1. Loop through the two lists using two different pointers.
2. Start with two intervals, one from each of the two lists and check if there is an overlap between them:
   1. If the starting point of the second interval is earlier than the first, swap them to ensure the first interval starts earlier or at the same time as the second.
   2. If the second interval starts before or when the first interval ends, there’s an overlap. The intersection of the two intervals is determined by `start = max(first.start, second.start), end = min(first.end, second.end)`.
3. If there’s an overlap, store the intersecting interval in a list of intersections.
4. Move to the next interval from the list whose current interval finishes first. By advancing the pointer in the list with the earlier end time, the algorithm efficiently skips intervals that can't possibly have any more overlaps with the other list.
5. Continue checking and advancing through the intervals from both lists until all possible comparisons are made.

Complexity analysis:

- Time complexity: O(N + M)
- Space complexity: O(N + M)

Where:

- `N` is the length of the first list.
- `M` is the length of the second list.

```python
def getIntersection(interval_1: List[int], interval_2: List[int]) -> Optional[Tuple[int, int]]:
    first_started = interval_1
    second_started = interval_2

    if second_started[0] < first_started[0]:
        first_started, second_started = second_started, first_started

    if second_started[0] <= first_started[1]:
        return (
            max(first_started[0], second_started[0]),
            min(first_started[1], second_started[1])
        )

    return None


def intervalIntersection(firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
    intersections = []

    i = 0
    j = 0
    while i < len(firstList) and j < len(secondList):
        interval1 = firstList[i]
        interval2 = secondList[j]

        intersection = getIntersection(interval1, interval2)
        if intersection:
            intersections.append(intersection)

        # move next from the interval which finishes first
        if interval1[1] < interval2[1]:
            i += 1
        elif interval2[1] < interval1[1]:
            j += 1
        else:
            i += 1
            j += 1

    return intersections
```
