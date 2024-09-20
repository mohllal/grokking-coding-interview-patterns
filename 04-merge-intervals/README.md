# Pattern: Merge Intervals

The merge interval pattern describes an efficient technique to deal with overlapping intervals. In a lot of problems involving intervals, we either need to find overlapping intervals or merge intervals if they overlap.

Given two intervals (`a` and `b`), there will be six different ways the two intervals can relate to each other:

1. No overlap:
   1. `a` starts before `b`, `a.end < b.start` (diagram 1).
   2. `b` starts before `a`, `b.end < a.start` (diagram 6).
2. Partial overlap:
   1. `a` starts before `b` but ends after `b` starts, `a.start < b.start` and `a.end >= b.start` and `a.end < b.end` (diagram 2).
   2. `b` starts before `a` but ends after `a` starts, `b.start < a.start` and `b.end >= a.start` and `b.end < a.end` (diagram 4).
3. Full overlap:
   1. `a` fully overlaps `b`, `a.start <= b.start` and `a.end >= b.end` (diagram 3).
   2. `b` fully overlaps `a`, `b.start <= a.start` and `b.end >= a.end` (diagram 5).

![Pattern: Merge Intervals](../images/pattern-merge-intervals.png)

_Credit: [Coding Interview Pattern: Merge Interval](https://medium.com/codex/grokking-the-coding-interview-pattern-merge-interval-6e6b1e9e038c)_

## Problems

|                           Problem                            |       Complexity        |
| :----------------------------------------------------------: | :---------------------: |
|          [Merge Intervals](./01-merge-intervals.md)          |     :star2: :star2:     |
|        **[Insert Interval](./02-insert-interval.md)**        |     :star2: :star2:     |
|   [Intervals Intersection](./03-intervals-intersection.md)   |     :star2: :star2:     |
| [Conflicting Appointments](./04-conflicting-appointments.md) |     :star2: :star2:     |
|  **[Minimum Meeting Rooms](./05-minimum-meeting-rooms.md)**  | :star2: :star2: :star2: |
