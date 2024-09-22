# Problem: Reverse a Linked List

LeetCode problem: [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/).

Given the head of a singly linked list, reverse the linked list in place. Write a function to return the new head of the reversed linked list.

## Examples

Example 1:

```plaintext
Input: 2 ──▶ 4 ──▶ 6 ──▶ 8 ──▶ 10 ──▶ null
Output: 10 ──▶ 8 ──▶ 6 ──▶ 4 ──▶ 2 ──▶ null
```

## Solution 1

The algorithm reverses the linked list iteratively. It starts with initiating a variable `current` which will initially point to the `head` of the linked list and a variable `previous` which will point to the previous node that we have processed; initially `previous` will point to `null`.

In a stepwise manner, the algorithm reverse the `current` node by pointing it to the `previous` before moving on to the next node. Also, it updates the `previous` to always point to the previous node that we have processed.

Here is the visual representation of our algorithm:

Iteration 1:

```plaintext
Iteration 1:

before: current = 2, previous = null
2 (current) ──▶ 4 ──▶ 6 ──▶ 8 ──▶ 10 ──▶ null

after: current = 4, previous = 2
null ◀── 2 (previous) //// 4 (current) ──▶ 6 ──▶ 8 ──▶ 10 ──▶ null
```

Iteration 2:

```plaintext
before: current = 4, previous = 2
null ◀── 2 (previous) //// 4 (current) ──▶ 6 ──▶ 8 ──▶ 10 ──▶ null

after: current = 6, previous = 4
null ◀── 2 ◀── 4 (previous) //// 6 (current) ──▶ 8 ──▶ 10 ──▶ null
```

Iteration 3:

```plaintext
before: current = 6, previous = 4
null ◀── 2 ◀── 4 (previous) //// 6 (current) ──▶ 8 ──▶ 10 ──▶ null

after: current = 8, previous = 6
null ◀── 2 ◀── 4 ◀── 6 (previous) //// 8 (current) ──▶ 10 ──▶ null
```

Iteration 4:

```plaintext
before: current = 8, previous = 6
null ◀── 2 ◀── 4 ◀── 6 (previous) //// 8 (current) ──▶ 10 ──▶ null

after: current = 10, previous = 8
null ◀── 2 ◀── 4 ◀── 6 ◀── 8 (previous) //// 10 (current) ──▶ null
```

Iteration 5:

```plaintext
before: current = 10, previous = 8
null ◀── 2 ◀── 4 ◀── 6 ◀── 8 (previous) //// 10 (current) ──▶ null

after: current = null, previous = 10
null ◀── 2 ◀── 4 ◀── 6 ◀── 8 ◀── 10 (previous) //// null (current)
```

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(head: Optional[ListNode]) -> Optional[ListNode]:
    current_node = head
    previous_node = None
    
    while current_node is not None:
        next_node = current_node.next
        
        current_node.next = previous_node
        previous_node = current_node

        current_node = next_node
    
    return previous_node
```

## Solution 2

The algorithm reverses the linked list recursively. It starts from the head of the linked list and recursively processes each subsequent node until it reaches the last node (base case). As the recursion unwinds, it reverses the pointers between adjacent nodes.

The algorithm follows these steps:

1. The base case handles when the linked list is either empty or contains only one node, returning that node as the new head.
2. In the recursive step, the function calls itself with the `next` node in the linked list until it reaches the last node.
3. Once recursion reaches the last node, the pointers are flipped to reverse the link between each pair of nodes. The `current` node’s `next` node is made to point back to the `current` node, and the `current` node’s `next` pointer is set to `None` (indicating the end of the reversed list).
4. Finally, the new head of the reversed linked list is returned as the recursive calls complete.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(N)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(current: Optional[ListNode]) -> Optional[ListNode]:
    if current is None or current.next is None:
        return current

    reversed_head = reverseList(current.next)

    # reverse the pointers of the current node and its next node
    # current setup is: 1 (current) -> 2 (current.next) -> ..., after reversal: 2 -> 1 -> null
    current.next.next = current # make the next node point back to the current node
    current.next = None # set the current node's next to None (end of the reversed part)

    return reversed_head
```
