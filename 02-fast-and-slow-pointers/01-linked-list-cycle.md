# Problem: Linked List Cycle

LeetCode problem: [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/).

Given the head of a singly linked list, write a function to determine if the linked list has a cycle in it or not.

## Examples

Example 1:

```plaintext
Input: 0 ──▶ 1 ──▶ 3 ──▶ (2) ──▶ 4
                          ▲       │
                          │       │
                          │       ▼
                          └───── (cycle)
Explanation: Linked list with a cycle
```

Example 2:

```plaintext
Input: 0 ──▶ 1 ──▶ 3 ──▶ 2 ──▶ 4 ──▶ null
Explanation: Linked list without a cycle
```

## Solution

Two pointers are used:

1. Slow pointer: Moves one node at a time.
2. Fast pointer: Moves two nodes at a time.

This leads to two possible outcomes:

- If the linked list doesn’t have a cycle in it, the fast pointer will reach the end of the linked list before the slow pointer to reveal that there is no cycle.
- If the linked list doesn’t have a cycle in it, the fast pointer will eventually catch up with the slow pointer after making a full cycle pass.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def hasCycle(head: Optional[ListNode]) -> bool:
    slow = head
    fast = head
    
    while fast is not None and fast.next is not None:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False
```

## Similar Problem

Given the head of a singly linked list with a cycle, find the length of the cycle.

### Examples

Example 1:

```plaintext
Input: 0 ──▶ 1 ──▶ 3 ──▶ (2) ──▶ 4
                          ▲       │
                          │       │
                          │       ▼
                          └───── (cycle)
Output: 2
Explanation: Linked list with a cycle of two nodes
```

Example 2:

```plaintext
Input: 0 ──▶ 1 ──▶ 3 ──▶ 2 ──▶ 4 ──▶ null
Output: 0
Explanation: Linked list without a cycle
```

### Solution

We can use the above solution to find the cycle in the linked list. Once the fast and slow pointers meet, we can save the slow pointer and iterate the whole cycle with another pointer until we both pointers catch up again to find the length of the cycle.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def getLength(pointer: ListNode) -> int:
    current = pointer.next
    length = 1

    while current != pointer:
        current = current.next
        length += 1
    
    return length

def calculateCycleLength(head: Optional[ListNode]) -> bool:
    slow = head
    fast = head
    
    while fast is not None and fast.next is not None:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return cycleLength(slow)

    return 0
```
