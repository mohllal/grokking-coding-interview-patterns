# Problem: Rearrange a Linked List

LeetCode problem: [143. Reorder List](https://leetcode.com/problems/reorder-list/).

Given the head of a singly linked list, write a method to modify the linked list such that the nodes from the second half of the linked list are inserted alternately to the nodes from the first half in reverse order.

Your algorithm should not use any extra space and the input linked list should be modified in-place.

## Examples

Example 1:

```plaintext
Input: 2 ──▶ 4 ──▶ 6 ──▶ 8 ──▶ 10 ──▶ 12 ──▶ null
Output: 2 ──▶ 12 ──▶ 4 ──▶ 10 ──▶ 6 ──▶ 8 ──▶ null 
```

Example 2:

```plaintext
Input: 2 ──▶ 4 ──▶ 6 ──▶ 8 ──▶ 10 ──▶ null
Output: 2 ──▶ 10 ──▶ 4 ──▶ 8 ──▶ 6 ──▶ null
```

## Solution

This problem shares similarities with the [Palindrome Linked List](./05-palindrome-linked-list.md) problem.

The algorithm follows these steps:

1. Identify the middle node of the linked list. This is similar to solving the [Middle of the Linked List](./04-middle-of-the-linked-list.md) problem.
2. Reverse the second half of the linked list.
3. Iterate through the first half and the reversed second half to produce a linked list in the required order.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# O(N) time and O(1) space
def reverseList(node: Optional[ListNode]) -> Optional[ListNode]:
    prev_node = None
    current = node
    
    while current is not None:
        next_node = current.next
        
        current.next = prev_node
        prev_node = current
        
        current = next_node
        
    return prev_node

# O(N) time and O(1) space
def getMiddleNode(node: Optional[ListNode]) -> Optional[ListNode]:
    slow = node
    fast = node
    
    while fast is not None and fast.next is not None:
        slow = slow.next
        fast = fast.next.next
        
    return slow

def reorderList(head: Optional[ListNode]) -> None:
  second_half = getMiddleNode(head)
  reversed_second_half = reverseList(second_half)
      
  pointer1 = head
  pointer2 = reversed_second_half
  while pointer1 is not None and pointer2 is not None:
      pointer1_next = pointer1.next
      pointer2_next = pointer2.next
      
      # preventing cycle
      if pointer1.next != pointer2:
          pointer1.next = pointer2
          pointer2.next = pointer1_next       
    
      pointer1 = pointer1_next
      pointer2 = pointer2_next
  
  return head
```
