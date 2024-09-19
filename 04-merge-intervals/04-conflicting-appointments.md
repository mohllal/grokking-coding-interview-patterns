# Problem: Conflicting Appointments

LeetCode problem: [252. Meeting Rooms](https://leetcode.com/problems/meeting-rooms/).

Given an array of intervals representing ‘N’ appointments, find out if a person can attend all the appointments.

## Examples

Example 1:

```plaintext
Appointments: [[1,4], [2,5], [7,9]]
Output: false
Explanation: Since [1,4] and [2,5] overlap, a person cannot attend both of these appointments.
```

Example 2:

```plaintext
Appointments: [[6,7], [2,4], [8,12]]
Output: true
Explanation: None of the appointments overlap, therefore a person can attend all of them.
```

Example 3:

```plaintext
Appointments: [[4,5], [2,3], [3,6]]
Output: false
Explanation: Since [4,5] and [3,6] overlap, a person cannot attend both of these appointments.
```

## Solution

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(1)

```python
def canAttendMeetings(intervals: List[List[int]]) -> bool:
    intervals.sort(key=lambda interval: interval[0])
    
    for i in range(1, len(intervals)):
        prev_interval = intervals[i - 1]
        curr_interval = intervals[i]
        
        # intervals having condition a.start == b.end
        # doesn't represent conflicting appointments as one starts right after the other
        if curr_interval[0] < prev_interval[1]:
            return False
    
    return True
```

## Similar Problem

Given a list of appointments, find all the conflicting appointments.

### Examples

Example 1:

```plaintext
Appointments: [[4,5], [2,3], [3,6], [5,7], [7,8]]
Output: [[[4, 5], [3, 6]], [[3,6] and [5,7]]]
Explanation:
[4,5] and [3,6] conflict.
[3,6] and [5,7] conflict.
```

### Solution

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

```python
def conflictingAppointments(intervals: List[List[int]]) -> bool:
    conflicts = []
    intervals.sort(key=lambda interval: interval[0])
  
    for i in range(1, len(intervals)):
        prev_interval = intervals[i - 1]
        curr_interval = intervals[i]
        
        # intervals having condition a.start == b.end
        # doesn't represent conflicting appointments as one starts right after the other
        if curr_interval[0] < prev_interval[1]:
            conflicts.append([prev_interval, curr_interval])
    
    return conflicts
```
