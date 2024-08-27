# Problem: Palindrome Linked List

LeetCode problem: [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/).

Given the head of a singly linked list, write a method to check if the linked list is a palindrome or not.

Your algorithm should use constant space and the input linked list should be in the original form once the algorithm is finished. The algorithm should have `O(N)` time complexity where `N` is the number of nodes in the linked list.

## Examples

Example 1:

```plaintext
Input: 2 ──▶ 4 ──▶ 6 ──▶ 4 ──▶ 2 ──▶ null
Output: True
```

Example 2:

```plaintext
Input: 2 ──▶ 4 ──▶ 6 ──▶ 4 ──▶ 2 ──▶ 2 ──▶ null
Output: false
```

## Solution

To determine if a linked list is a palindrome,  we can approach the problem by splitting the linked list into two halves. The key idea is to verify whether the node values of the first half in the forward direction mirror the node values of the second half in the backward direction.

The algorithm follows these steps:

1. Identify the middle node of the linked list. This is similar to solving the [Middle of the Linked List](./04-middle-of-the-linked-list.md) problem.
2. Reverse the second half of the linked list.
3. Compare the values of the nodes in the first half with the reversed second half to check for palindrome.
4. Reverse the second half of the linked list again to revert and bring it back to its original form.

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

def isPalindrome(head: Optional[ListNode]) -> bool:
    second_half = getMiddleNode(head)
    reversed_second_half = reverseList(second_half)
    
    pointer1 = head
    pointer2 = reversed_second_half
    while pointer1 is not None and pointer2 is not None:
        if pointer1.val != pointer2.val:
            break

        pointer1 = pointer1.next
        pointer2 = pointer2.next

    reverseList(reversed_second_half)
    return True if pointer2 is None else False
```
