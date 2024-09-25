# Problem: Reverse a Sub-List

LeetCode problem: [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/).

Given the head of a linked list and two positions ‘p’ and ‘q’, reverse the LinkedList from position ‘p’ to ‘q’.

## Examples

Example 1:

```plaintext
Input: Linked List = 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ null, p = 2, q = 4
Output: 1 ──▶ 4 ──▶ 3 ──▶ 2 ──▶ 5 ──▶ null
```

## Solution

The general idea behind the algorithm is to reverse a specific portion of a linked list between two given positions (`left` and `right`), while keeping the rest of the list intact.

The algorithm follows these steps:

1. Identify the start and end of the section to reverse.
   - The node just before the reversal starts (`node_before_start`)
   - The first node of the sub-list to reverse (`start_node`)
   - The last node of the sub-list to reverse (`end_node`)
   - The node immediately after the sub-list (`node_after_end`)
2. Once the sub-list boundaries are identified (between `left` and `right`), the algorithm reverses the connections between the nodes in this range.
3. Once the sub-list is reversed, reconnect it with the rest of the list by adjusting pointers.
   - It connects the node before the reversed sub-list (`node_before_start`) to the new head of the reversed portion (`end_node`).
   - It also connects the original start of the reversed sub-list (`start_node`) to the node immediately following the reversed section (`node_after_end`)
   - If the reversal starts from the head of the list (`left == 1`), the algorithm adjusts the `head` pointer to reflect the new head (`end_node`)
4. The modified list is returned, with the sub-list between positions `left` and `right` reversed while the rest of the list remains unchanged.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseBetween(head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
    if not head or left == right:
        return head

    node_before_start = None  # this will point to the node just before the 'left' position
    start_node = None         # this will point to the node at the 'left' position
    node_after_end = None     # this will point to the node right after the 'right' position
    end_node = None           # this will point to the node at the 'right' position

    current_position = 1
    previous_node = None
    current_node = head
    while current_node is not None:
        next_node = current_node.next

        if current_position == left:
            node_before_start = previous_node # node just before the 'left' position
            start_node = current_node # node at the 'left' position
        elif current_position == right:
            end_node = current_node # node at the 'right' position
            node_after_end = next_node # node just after the 'right' position

        # reverse the links between 'left' and 'right' positions
        if current_position >= left and current_position <= right:
            current_node.next = previous_node

        # move forward in the list
        previous_node = current_node
        current_node = next_node
        current_position += 1

    # reconnect the reversed portion with the rest of the list
    # node_before_start -> (end_node) -> .. -> (start_node) -> node_after_end

    # if there's a node before the start, link it to the new head of the reversed sub-list
    if node_before_start:
        node_before_start.next = end_node
    else:
        head = end_node  # if reversing from the first node, adjust the head to the new start

    # link the original start node to the node after the end of the reversed portion
    start_node.next = node_after_end

    return head
```
