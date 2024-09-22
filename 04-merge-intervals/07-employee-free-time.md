# Problem: Employee Free Time

LeetCode problem: [759. Employee Free Time](https://leetcode.com/problems/employee-free-time/).

For K employees, we are given a list of intervals representing the working hours of each employee. Our goal is to find out if there is a free interval that is common to all employees.

You can assume that each list of employee working hours is sorted on the start time.

## Examples

Example 1:

```plaintext
Input: Employee Working Hours=[[[1,3], [5,6]], [[2,3], [6,8]]]
Output: [3,5]
Explanation: Both the employees are free between [3,5].
```

Example 2:

```plaintext
Input: Employee Working Hours=[[[1,3], [9,12]], [[2,4]], [[6,8]]]
Output: [4,6], [8,9]
Explanation: All employees are free between [4,6] and [8,9].
```

Example 3:

```plaintext
Input: Employee Working Hours=[[[1,3]], [[2,4]], [[3,5], [7,9]]]
Output: [5,7]
Explanation: All employees are free between [5,7].
```

## Solution 1

The algorithm flatten the list of employee schedules into one single list, merges the overlapping intervals, and then finds the gaps (or free time) between these merged intervals. The result is a list of time intervals where no employee is busy, which represents the collective free time.

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

Where:

- `N` is the total number of intervals `K * M`:
  - `K` is the number of employees
  - `M` is the max number of intervals per employee

```python
def employeeFreeTime(schedules: List[List[List[int]]]) -> List[List[int]]:
    # flatten the schedules into a single list of intervals
    intervals = [interval for schedule in schedules for interval in schedule]
    intervals.sort(key=lambda interval: interval[0])
    
    # merge overlapping intervals
    merged = []
    prev_interval = intervals[0]
    for i in range(1, len(intervals)):
        curr_interval = intervals[i]
        
        if curr_interval[0] <= prev_interval[1]:
            prev_interval[1] = max(prev_interval[1], curr_interval[1])
        else:
            merged.append(prev_interval)
            prev_interval = curr_interval
    
    merged.append(prev_interval)

    # find the gaps between the merged intervals (these are the free time periods)
    result = []
    for i in range(1, len(merged)):
        if merged[i][1] != merged[i - 1][0]:
            result.append([merged[i - 1][1], merged[i][0]])
    
    return result
```

## Solution 2

One fact about the previous approach is that we are not utilizing is that each employee list is individually sorted!

The algorithm uses a min-heap (priority queue) to always process the earliest interval across all employees. This ensures intervals are merged or compared in the correct chronological order.

The algorithm follows these steps:

1. Initialize a min-heap with the first (earliest) interval of every employee schedule.
2. Pop the smallest start-time interval, and then compare it with the next smallest start-time interval (again from the heap) to find if there is a gap.
3. If the two intervals overlap, they are merged.
4. If they don't overlap, the gap (free time) between them is added to the result.
5. After processing an interval, the algorithm checks if the employee has more intervals in their schedule. If so, the next interval is added to the heap.

Complexity analysis:

- Time complexity: O(N * LogK)
- Space complexity: O(K)

Where:

- `N` is the total number of intervals `K * M`:
  - `K` is the number of employees
  - `M` is the max number of intervals per employee

Why time complexity is O(N * LogK) and space complexity is O(K)?

The algorithm iterates through the intervals only once, which will take O(N), and every time we process an interval, we remove (and can insert) one interval in the heap, which will take O(logK).

At any time the heap will not have more than `K` elements.

```python
import heapq

def employeeFreeTime(schedules: List[List[List[int]]]) -> List[List[int]]:
    # initialize a min-heap to track the current smallest interval across all schedules
    min_heap = []
    
    # add the first interval of each employee's schedule to the min-heap
    for i in range(len(schedules)):
        # (interval, employee_index, interval_index)
        heapq.heappush(min_heap, (schedules[i][0], i, 0)) 
        
    result = []
    prev_interval = min_heap[0][0]
    while len(min_heap) > 0:
        # get the smallest (earliest) interval from the heap
        heap_top = heapq.heappop(min_heap)

        # (interval, employee_index, interval_index)
        curr_interval = heap_top[0]
        employee_schedule_index = heap_top[1]
        employee_interval_index = heap_top[2]
        
        # check if the current interval overlaps with the previous one
        if curr_interval[0] <= prev_interval[1]:
            # if there's an overlap, merge by extending the previous interval to cover this one
            prev_interval[1] = max(prev_interval[1], curr_interval[1])
        else:
            # if no overlap, the gap between intervals is a free time period
            result.append([prev_interval[1], curr_interval[0]])
            prev_interval = curr_interval
        
        # get the employee's schedule to see if they have more intervals to process
        employee_schedule = schedules[employee_schedule_index]
        
        # if the employee has more intervals, push the next one onto the heap
        if len(employee_schedule) > employee_interval_index + 1:
            heapq.heappush(min_heap, (
                schedules[employee_schedule_index][employee_interval_index + 1],
                employee_schedule_index,
                employee_interval_index + 1
            ))
    
    return result
```
