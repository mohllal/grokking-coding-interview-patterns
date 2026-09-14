---
title: Fruits into Baskets
difficulty: Medium
leetcode_title: Fruit Into Baskets
leetcode: https://leetcode.com/problems/fruit-into-baskets/
tags:
  - Array
  - Hash Table
  - Sliding Window
---

# Fruits into Baskets

## Problem description

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits`, where `fruits[i]` is the type of fruit the `i`-th tree produces.

You want to collect as much fruit as possible, subject to the owner's rules:

- You only have two baskets, and each basket can hold only a single type of fruit. There is no limit on how much fruit a basket can hold.
- Starting from any tree of your choice, you must pick exactly one fruit from **every** tree while moving to the right, including the tree you start at.
- Once you reach a tree whose fruit cannot fit in either basket, you must stop.

Return the maximum number of fruits you can pick.

## Examples

**Example 1:**

```plaintext
Input: fruits = ['A', 'B', 'C', 'A', 'C']
Output: 3
Explanation: We can put 2 'C' in one basket and one 'A' in the other, from the subarray ['C', 'A', 'C'].
```

**Example 2:**

```plaintext
Input: fruits = ['A', 'B', 'C', 'B', 'B', 'C']
Output: 5
Explanation: We can put 3 'B' in one basket and two 'C' in the other.
This can be done if we start with the second letter: ['B', 'C', 'B', 'B', 'C'].
```

## Constraints

- `1 <= len(fruits) <= 10^5`
- `0 <= fruits[i] < len(fruits)`

## Hints

<details>
<summary>Hint 1</summary>

Strip away the story. You must pick from consecutive trees, and you may hold at most two kinds of fruit. What is that asking for, in terms of subarrays?

</details>

<details>
<summary>Hint 2</summary>

"Two baskets" is a bound on how many *distinct* values the window may contain. You have already solved this with `k` as a parameter.

</details>

## Solution

### Intuition

The rules describe a contiguous run of trees containing at most two distinct fruit types — so this is the longest-window problem with the distinct-value budget fixed at two. The baskets are the frequency map; the "you must stop" rule is the window becoming invalid.

Grow the window by picking from the next tree. When a third fruit type appears, abandon trees from the left until only two types remain.

```plaintext
fruits = ['A', 'B', 'C', 'B', 'B', 'C']

A            {A:1}              valid     picked 1
AB           {A:1, B:1}         valid     picked 2
ABC          {A:1, B:1, C:1}    3 types, drop from the left
 BC          {B:1, C:1}         valid     picked 2
 BCB         {B:2, C:1}         valid     picked 3
 BCBB        {B:3, C:1}         valid     picked 4
 BCBBC       {B:3, C:2}         valid     picked 5   <- best

answer = 5
```

### Algorithm

1. Add `fruits[window_end]` to the frequency map
2. While the map holds more than two fruit types, decrement the count of `fruits[window_start]`, remove the key if it reaches zero, and advance `window_start`
3. Record `window_end - window_start + 1` as a candidate for the most fruit picked

### Complexity analysis

- Time complexity: $O(n)$ — each tree is entered once and left at most once.
- Space complexity: $O(1)$ — the map holds at most three fruit types at any moment.

```python
from collections import Counter
from typing import List


class Solution:
    def total_fruit(self, fruits: List[int]) -> int:
        maximum_fruits = 0

        window_start = 0
        window_counter = Counter()
        for window_end in range(len(fruits)):
            window_counter[fruits[window_end]] += 1

            while len(window_counter) > 2:
                start_fruit = fruits[window_start]
                window_counter[start_fruit] -= 1

                if window_counter[start_fruit] == 0:
                    del window_counter[start_fruit]

                window_start += 1

            maximum_fruits = max(maximum_fruits, window_end - window_start + 1)

        return maximum_fruits
```

## Relationship to [Longest Substring with At Most Two Distinct Characters](./03.1-longest-substring-with-at-most-two-distinct-characters.md)

The same problem, stated without the farm. That one counts distinct characters in a string; this one counts distinct fruit types in an integer array. The loop, the shrink condition, and both complexity bounds are identical — only the names differ.

Both are the `k = 2` case of [Longest Substring with K Distinct Characters](./03-longest-substring-with-k-distinct-characters.md).
