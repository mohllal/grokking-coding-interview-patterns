# Problem: Reverse Every K-element Sub-List

LeetCode problem: [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/).

Given the head of a linked list and a number `k`, reverse every `k` sized sub-list starting from the head.

If, in the end, you are left with a sub-list with less than `k` elements, reverse it too.

## Examples

Example 1:

```plaintext
Input: Linked List = 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 5 ──▶ 6 ──▶ 7 ──▶ 8 ──▶ null, k = 3
Output: Linked List = 3 ──▶ 2 ──▶ 1 ──▶ 6 ──▶ 5 ──▶ 4 ──▶ 8 ──▶ 7 ──▶ null
```

## Solution

The problem is quite similar to the [Reverse a Sub-List](./03-reverse-every-k-element-sub-list.md) problem. The only difference is that we have to reverse all the sub-lists of size `k`.

We can use the same approach, starting with the first sub-list (i.e. `p=1`, `q=k`) and keep reversing all the sub-lists of size `k`.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

Why space complexity is O(N)?

The list is traversed only once in the main loop. While the reversal operation for each group of `O(K)`, each node in the list is reversed only once.

Therefore, even though the reversal function is called multiple times, the total work done across all reversals is proportional to `N`, as each node is processed exactly once.

This results in an overall time complexity of `O(N)` not `O(N^2)`.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# O(N) time and O(1) space
def reverseSubList(head: Optional[ListNode], start: int, end: int) -> Optional[ListNode]:
    if not head or start == end:
        return head
    
    node_before_start = None # this will point to the node just before the 'start' position
    start_node = None        # this will point to the node at the 'start' position
    node_after_end = None    # this will point to the node just after the 'end' position
    end_node = None          # this will point to the node at the 'end' position
    
    current_position = 1
    previous_node = None
    current_node = head
    while current_node is not None and current_position <= end:
        next_node = current_node.next

        if current_position == start:
            node_before_start = previous_node # node just before the 'start' position
            start_node = current_node # node at the 'start' position
        elif current_position == end:
            end_node = current_node # node at the 'end' position
            node_after_end = next_node # node just after the 'end' position
        
        # reverse the sub-list between 'start' and 'end' positions
        if current_position >= start and current_position <= end:
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
        # if reversing from the first node, adjust the head to the new start
        head = end_node
     
    start_node.next = node_after_end
    
    return head

def reverseKGroup(head: Optional[ListNode], k: int) -> Optional[ListNode]:
    # count the total number of nodes in the list
    count = 0
    current_node = head
    while current_node is not None:
        count += 1
        current_node = current_node.next
    
    # perform the reversing in groups of k
    current_position = 1
    current_node = head
    while current_position <= count:
        if current_position % k == 0:
            start = (current_position - k) + 1
            end = current_position
            head = reverseSubList(head, start, end)
        
        current_node = current_node.next
        current_position += 1
    
    # reverse the remaining nodes if fewer than k are left
    remaining_nodes = count % k
    if remaining_nodes > 0:
        start = count - remaining_nodes + 1
        end = count
        head = reverseSubList(head, start, end)

    return head
```
