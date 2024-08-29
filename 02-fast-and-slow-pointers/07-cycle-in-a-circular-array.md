# Problem: Cycle in a Circular Array

LeetCode problem: [457. Circular Array Loop](https://leetcode.com/problems/circular-array-loop/).

We are given an array containing positive and negative numbers. Suppose the array contains a number `M` at a particular index. Now, if `M` is positive we will move forward `M` indices and if `M` is negative move backwards `M` indices.

You should assume that the array is circular which means two things:

- If, while moving forward, we reach the end of the array, we will jump to the first element to continue the movement.
- If, while moving backward, we reach the beginning of the array, we will jump to the last element to continue the movement.

Write a function to determine if the array has a cycle. The cycle should have more than one element and should follow one direction which means the cycle should not contain both forward and backward movements.

## Examples

Example 1:

```plaintext
Input: [1, 2, -1, 2, 2]

Index: 0 -> 1 -> 2 -> 3 -> 4
Value: 1    2   -1    2    2

Linked list representation:

4     2
│     │
▼     ▼
0 ──▶ 1 ──▶ 3
▲           │
│           │
│           ▼
└───────── (cycle)

Output: true
Explanation: The array has a cycle among indices: 0 -> 1 -> 3 -> 0
```

Example 2:

```plaintext
Input: [2, -1, 1, 2, 2]

Index: 0 -> 1 -> 2 -> 3 -> 4
Value: 2   -1    1    2    2

Linked list representation:

1 ◀── 4
│
▼ 
0 ──▶ 2 ──▶ 3
▲           │
│           │
│           ▼
└───────── (cycle)

Output: true
Explanation: The array has a cycle among indices: 0 ──▶ 2 ──▶ 3 ──▶ 0
```

Example 3:

```plaintext
Input: [1, -1, 5, 1, 4]

Index: 0 -> 1 -> 2 -> 3 -> 4
Value: 1   -1    5    1    4

Linked list representation:

List 1:

0 ──f──▶ 1
▲        │
│        B
│        │
│        ▼              
└─── (cycle)

List 2:
 ──────────
│          │
2          │
│          │
(cycle) ◀───


List 3:

3 ──f──▶ 4
▲        │
│        f
│        │
│        ▼              
└─── (cycle)

Output: false
Explanation:

The array represents three linked lists as follows:

1. The cycle 0 ──▶ 1 ──▶ 0 ──▶ ..., and while it is of size > 1, it has a node jumping forward and a node jumping backward so it is not a valid cycle.
2. The cycle (2) has a length of 1 so it is not a valid cycle.
3. The cycle 3 ──▶ 4 ──▶ 3 ──▶ ..., has a valid length and all nodes within the cycle are jumping in the same direction.
```

## Solution 1

The algorithm begins by creating an array of linked list nodes to represent the circular array. Each node corresponds to an element in the array and has information about its direction (positive or negative as boolean value).

Next, it checks each node for a cycle using a cycle detection algorithm similar to the one used in the [Linked List Cycle](./01-linked-list-cycle.md) problem. The cycle detection ensures that if a cycle is found, the following conditions is true:

1. Elements within the cycle move in the same direction (by checking the direction of each element while searching for the cycle).
2. Cycle should have more than one element.

Complexity analysis:

- Time complexity: O(N^2)
- Space complexity: O(N)

```python
class ListNode:
    def __init__(self, value: int, direction: bool):
        self.value = value
        self.direction = direction
        self.next = None
    
    def __repr__(self):
        return f'ListNode(\'{self.value}\', {self.direction})'

# O(n) time and O(1) space
def hasCycleWithSameDirection(head: Optional[ListNode]) -> bool:
    slow = head
    fast = head
    
    while fast is not None and fast.next is not None:
        # skip self cycles (one-node length cycle)
        if slow == slow.next or fast == fast.next or fast.next == fast.next.next:
            break
        
        # skip cycles with mixed directions
        if not (slow.direction == fast.direction == fast.next.direction):
            break

        slow = slow.next
        fast = fast.next.next

        if slow == fast:
            return True
    
    return False

def circularArrayLoop(nums: List[int]) -> bool:
        nodes = []
        for i in range(len(nums)):
            direction = True if nums[i] >= 0 else False
            nodes.append(ListNode(i, direction))

        for i in range(len(nodes)):
            current_node = nodes[i]
            
            next_node_idx = (i + nums[i]) % len(nums)
            next_node = nodes[next_node_idx]
            
            current_node.next = next_node

        for i in range(len(nodes)):
            if hasCycleWithSameDirection(nodes[i]):
                return True
        
        return False
```

## Solution 2

This algorithm improves upon the previous version by introducing two key changes:

1. In-place cycle detection: instead of constructing a separate array of linked list nodes, the algorithm directly uses the input array to traverse and detect potential cycles that move in the same direction. This eliminates the need for additional space.
2. Marking visited elements: after each cycle detection attempt, all elements involved in that cycle (with the same direction) are marked as visited by setting their values to `0`. This prevents them from being rechecked in future iterations, ensuring that each element is processed at most twice — once during cycle detection and once during marking.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
# O(1) time and O(1) space
def getNextIndex(nums: List[int], index: int) -> int:
    return (index + nums[index]) % len(nums)

# O(1) time and O(1) space
def getDirection(nums: List[int], index: int) -> bool:
    return nums[index] > 0

# O(N) time and O(1) space
def hasCycleWithSameDirection(nums: List[int], index: int) -> bool:
    slow = index
    fast = index
    
    while True:
        slow_direction = getDirection(nums, slow)
        slow_next = getNextIndex(nums, slow)
        
        fast_direction = getDirection(nums, fast)
        fast_next = getNextIndex(nums, fast)
        fast_next_direction = getDirection(nums, fast_next)
        fast_next_next = getNextIndex(nums, fast_next)
        
        # skip self cycles (one-node length cycle)
        if slow == slow_next or fast == fast_next or fast_next == fast_next_next:
            break
        
        # skip cycles with mixed directions
        if not (slow_direction == fast_direction == fast_next_direction):
            break

        slow = slow_next
        fast = fast_next_next

        if slow == fast:
            return True
    
    return False

# O(N) time and O(1) space
def markCycleElementsAsVisited(nums: List[int], index: int) -> None:
    slow = index
    temp = nums[index]
    
    # ensure that we only continue moving through the cycle while the elements
    # we are visiting have the same direction as the starting element
    while nums[slow] * temp > 0:
        next_slow = getNextIndex(nums, slow)
        nums[slow] = 0  # mark as visited
        slow = next_slow

def circularArrayLoop(nums: List[int]) -> bool:
    for i in range(len(nums)):
        # skip visited elements
        if nums[i] == 0:
            continue

        if hasCycleWithSameDirection(nums, i):
            return True

        markCycleElementsAsVisited(nums, i)

    return False
```
