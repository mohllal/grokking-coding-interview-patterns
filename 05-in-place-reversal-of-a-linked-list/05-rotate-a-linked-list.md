# Problem: Rotate a Linked List

LeetCode problem: [61. Rotate List](https://leetcode.com/problems/rotate-list/).

Given the head of a singly linked list and a number `k`, rotate the linked list to the right by `k` nodes.

## Examples

Example 1

```plaintext
Input: Linked List = 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ 6 ──▶ null, k = 3
Output: Linked List = 4 ──▶ 5 ──▶ 6 ──▶ 1 ──▶ 2 ──▶ 3 ──▶ null
```

Example 2:

```plaintext
Input: Linked List = 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ null, k = 8
Output: Linked List = 3 ──▶ 4 ──▶ 5 ──▶ 1 ──▶ 2 ──▶ null
```

## Solution

The algorithms follows these steps:

1. Count the number of nodes in the linked list.
2. Calculate the effective rotation needed by taking `k % length`, as rotating a linked list by its length or a factor of its length doesn't change it.
3. After determining the effective rotation, it finds the new head and tail after rotation. The new head is the node at position `length - k + 1` (considering 1-based indexing), and the new tail is the node before the new head.
4. Finally, it breaks the list by setting the next tail next pointer to null and sets the new tail's next pointer to the head of the original list.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
    if not head or not head.next or k == 0:
        return head
    
    # count the length of the list and find the tail
    length = 1
    tail = head
    while tail.next is not None:
        length += 1
        tail = tail.next
        
    # if the number of rotations will revert back the linked list to its original form
    k = k % length
    if k == 0:
        return head
    
    # find the new head after rotation
    # new head will be at (length - k)th node, and new tail will be just before it
    new_tail_position = length - k
    new_tail = head
    for _ in range(new_tail_position - 1):
        new_tail = new_tail.next

    new_head = new_tail.next
    new_tail.next = None # # break the list
    tail.next = head  # connect the old tail of the list to the old head
    
    return new_head
```
