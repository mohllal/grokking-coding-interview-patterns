# Problem: Middle of the Linked List

LeetCode problem: [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/).

Given the head of a singly linked list, write a method to return the middle node of the linked list.

If the total number of nodes in the linked list is even, return the second middle node.

## Examples

Example 1:

```plaintext
Input: 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ null
Output: 3
```

Example 2:

```plaintext
Input: 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ 6 ──▶ null
Output: 4
```

Example 3:

```plaintext
Input: 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ 6 ──▶ 7 ──▶ null
Output: 4
```

## Solution 1

First calculates the total length of the list by traversing it, then iterates through the first half of the list to locate the middle node, which is then returned.

Complexity analysis:

- Time complexity: O(N) - two-pass algorithm
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def middleNode(head: Optional[ListNode]) -> Optional[ListNode]:
    length = 0
    current = head
    while current is not None:
        length += 1
        current = current.next

    middle = head
    for i in range(length // 2):
        middle = middle.next
    
    return middle
```

## Solution 2

Using two pointers starting at the beginning of the linked list:

- The slow pointer moves one node at a time.
- The fast pointer moves two nodes at a time.

When the fast pointer reaches the end of the linked list, the slow pointer will be positioned at the middle of the list.

Complexity analysis:

- Time complexity: O(N) - one-pass algorithm
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def middleNode(head: Optional[ListNode]) -> Optional[ListNode]:
    fast = head
    slow = head
    
    while fast is not None and fast.next is not None:
        fast = fast.next.next
        slow = slow.next
        
    return slow
```
