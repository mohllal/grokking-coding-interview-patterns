# Problem: Minimum Meeting Rooms

LeetCode problem: [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/).

Given a list of intervals representing the start and end time of ‘N’ meetings, find the minimum number of rooms required to hold all the meetings.

## Examples

Example 1:

```plaintext
Meetings: [[1,4], [2,5], [7,9]]
Output: 2
Explanation: Since [1,4] and [2,5] overlap, we need two rooms to hold these two meetings. [7,9] can occur in any of the two rooms later.
```

Example 2:

```plaintext
Meetings: [[6,7], [2,4], [8,12]]
Output: 1
Explanation: None of the meetings overlap, therefore we only need one room to hold all meetings.
```

Example 3:

```plaintext
Meetings: [[1,4], [2,3], [3,6]]
Output:2
Explanation: Since [1,4] overlaps with the other two meetings [2,3] and [3,6], we need two rooms to hold all the meetings.
```

Example 4:

```plaintext
Meetings: [[4,5], [2,3], [2,4], [3,5]]
Output: 2
Explanation: We will need one room for [2,3] and [3,5], and another room for [2,4] and [4,5].
```

## Solution 1

Counting the number of overlapping intervals alone doesn't give us the correct number of rooms needed. Even if three intervals `x`, `y`, and `z` overlap, i.e. `x` overlaps with `y` and `y` overlaps with `z`, the condition `z.start >= x.end` implies that we can reuse the same room for intervals x and z. Thus, we only need 2 rooms, not 3, as `z` can be scheduled in the same room after `x` finishes.

For example, consider the intervals `[[2,3], [2,4], [3,5]]`.

In this case, some meetings are mutually exclusive. For instance, `[2,3]` and `[3,5]` do not overlap, so they can be scheduled in the same room, even though all three intervals appear to overlap.

To solve this problem accurately, we need to account for the mutual exclusivity of overlapping meetings:

1. Sort the meetings by their start times.
2. Schedule the first meeting (let’s call it `m1`) in a room (`r1`).
3. If the next meeting `m2` doesn't overlap with `m1`, it can be scheduled in the same room (`r1`).
4. If `m2` and the next meeting `m3` overlap, we cannot use `r1` for `m3`. Instead, we schedule it in a new room (`r2`).
5. For any subsequent meeting (say `m4`), if it overlaps with `m3`, we check if room `r1` is available by comparing the end times of the meetings already scheduled. If the end time of `m2` (in `r1`) is before the start time of `m4`, we can reuse `r1`. Otherwise, we need to schedule `m4` in another room (`r3`).

To efficiently manage this process, we need to track the end times of meetings currently in progress. A Min Heap is ideal for this, as it allows us to quickly find the meeting with the earliest end time, ensuring we can reuse rooms as soon as they become available.

The algorithm follows these steps:

1. Sort the meetings by their start times.
2. Use a min-heap to track all ongoing meetings. The heap will help find the meeting with the earliest end time among those currently happening.
3. Iterate through each meeting. For each meeting (let’s call it `m1`), it will attempt to schedule it.
4. Remove all meetings from the heap that have ended before `m1` starts. This step ensures that only currently active meetings remain in the heap. Specifically, remove any meeting whose end time is less than or equal to the start time of `m1`.
5. Add `m1` to the heap to represent it as an ongoing meeting.
6. The heap now contains all the overlapping meetings. The size of the heap at any point represents the number of rooms required. Keep track of the largest heap size, as that will indicate the minimum number of rooms needed.

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

Why time complexity is O(N * LogN)?

Sorting the array of meetings takes O(N * LogN), where N is the number of meetings. Additionally, as we iterate through the meetings, we may need to push or pop elements from the min-heap (priority queue), and each of these operations takes O(LogN). Since we perform these operations for each meeting during the loop, the overall time complexity becomes O(N * LogN).

```python
import heapq

def minMeetingRooms(intervals: List[List[int]]) -> bool:
    intervals.sort(key=lambda interval: interval[0])
    
    rooms = 0
    min_heap = [] # min heap which represents all ongoing meetings
    for i in range(len(intervals)):
        # remove all the meetings that have ended
        while len(min_heap) > 0 and intervals[i][0] >= min_heap[0]:
            heapq.heappop(min_heap)
        
        # push end date for the current meeting into the min heap
        heapq.heappush(min_heap, intervals[i][1])
        
        # all ongoing meetings are in the min heap, so we need rooms for all of them
        rooms = max(rooms, len(min_heap))
        
    return rooms
```

## Solution 2

The algorithm revolves around identifying the start and end times of meetings, treating each of them as an event. If a meeting starts, it adds one more room. If a meeting ends, it frees up a room.

Let's take an example:

```plaintext
Intervals: [[1,5], [2,6], [4,8]]

Time: 1   2   3   4   5   6   7   8
      [-- meeting 1 --]
          [-- meeting 2 --]
                  [-- meeting 3 --]
```

At time 4, all three meetings overlap, which means we need 3 rooms at that moment.

The algorithm follows these steps:

1. Separate meeting start and end events:
    1. Think of the start time as the moment we “need a room”.
    2. The end time is when we “free up a room”.
2. Sort events by time and if two events happen at the same time, it prioritizes the end event (free up a room before assigning a new one).
3. As it progress through the timeline of these meetings, it tracks how many rooms are in use at every moment. Each time a meeting starts, it increases the room count by 1. Each time a meeting ends, it decreases the count by 1.
4. Keep track of the maximum number of rooms needed at any point in time because it represents the peak number of simultaneous meetings.

Complexity analysis:

- Time complexity: O(N * LogN)
- Space complexity: O(N)

```python
def minMeetingRooms(intervals: List[List[int]]) -> bool:
    events = []
    for interval in intervals:
        events.append([interval[0], 1])
        events.append([interval[1], 0])

    events.sort(key=lambda event: event[0])
    
    rooms = 0
    count = 0
    for event in events:
        if event[1] == 1:
            count += 1
        else:
            count -= 1
        rooms = max(rooms, count)

    return rooms
```
