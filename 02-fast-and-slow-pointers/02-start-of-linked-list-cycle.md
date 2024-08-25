# Problem: Start of Linked List Cycle

LeetCode problem: [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/).

Given the head of a singly linked list that contains a cycle, write a function to find the starting node of the cycle.

## Examples

Example 1:

```plaintext
Input: 1 ──▶ 2 ──▶ (3) ──▶ 4 ──▶ 5 ──▶ 6
                    ▲                  │
                    │                  │
                    │                  ▼
                    └──────────────── (cycle)
Output: Node(3)
```

Example 2:

```plaintext
Input: (1) ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ 6
        ▲                              │
        │                              │
        │                              ▼
        └──────────────────────────── (cycle)
Output: Node(1)
```

## Solution

Two pointers are used:

1. Slow pointer: Moves one node at a time.
2. Fast pointer: Moves two nodes at a time.

If the two pointers meet, a cycle is detected. To identify the starting node of the cycle, we apply the [Floyd's cycle detection algorithm](https://en.wikipedia.org/wiki/Cycle_detection#Floyd's_tortoise_and_hare) to find the the starting node of the cycle by moving one pointer to the head of the list while the other remains at the meeting point, and move them one node at a time, they will meet up again at the node where the cycle begins.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def detectCycle(head: Optional[ListNode]) -> Optional[ListNode]:
    slow = head
    fast = head

    while fast is not None and fast.next is not None:
        slow = slow.next
        fast = fast.next.next
        
        # cycle detected
        if slow == fast:
            # Floyd's cycle detection algorithm
            slow = head
            while slow != fast:
                slow = slow.next
                fast = fast.next
            
            return slow

    return None
```

## Similar Problem

LeetCode problem: [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/).

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only one repeated number in `nums`, return this repeated number.
You must solve the problem without modifying the array `nums` and uses only constant extra space.

### Examples

Example 1:

```plaintext
Input: [1,3,4,2,2]
Output: 2
```

Example 2:

```plaintext
Input: [3,1,3,4,2]
Output: 3
```

### Solution

We can think of the array as representing a linked list where each element points to another element in the array by its index. Specifically, each value in the array indicates the index of the next element to jump to, forming a sequence like `index -> value -> index`. This is possible because the array's values are guaranteed to be within the range `[1, n]`.

Given that there is one duplicate number, this linked list representation will eventually form a cycle. In other words, two different indices in the array will point to the same value, creating a loop.

The duplicate number will always be the starting point of the cycle, which we can use the [Floyd's cycle detection algorithm](https://en.wikipedia.org/wiki/Cycle_detection#Floyd's_tortoise_and_hare) to find it using two pointers: a slow pointer and a fast pointer. Once the two pointers meet within the cycle, resetting one pointer to the start of the array and moving both pointers one step at a time will lead them to the beginning of the cycle, which is the duplicate number.

```plaintext
Input: [1,3,4,2,2]

Index: 0 -> 1 -> 2 -> 3 -> 4
Value: 1    3    4    2    2

Linked list representation:

0 ──▶ 1 ──▶ 3 ──▶ (2) ──▶ 4
                   ▲      │
                   │      │
                   │      ▼
                   └──── (cycle)
Output: 2
```

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def findDuplicate(nums: List[int]) -> int:
    slow = 0
    fast = 0
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        
        if slow == fast:
            break
    
    slow = 0
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    
    return slow
```

## References

- [Floyd's cycle detection algorithm](https://www.youtube.com/watch?v=S5TcPmTl6ww)