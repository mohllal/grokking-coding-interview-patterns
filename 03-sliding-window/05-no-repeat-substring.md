---
title: No-repeat Substring
difficulty: Medium
leetcode_title: Longest Substring Without Repeating Characters
leetcode: https://leetcode.com/problems/longest-substring-without-repeating-characters/
tags:
  - Hash Table
  - String
  - Sliding Window
---

# No-repeat Substring

## Problem description

Given a string, find the length of the longest substring which has no repeating characters.

## Examples

**Example 1:**

```plaintext
Input: s = "aabccbb"
Output: 3
Explanation: The longest substring without any repeating characters is "abc".
```

**Example 2:**

```plaintext
Input: s = "abbbb"
Output: 2
Explanation: The longest substring without any repeating characters is "ab".
```

**Example 3:**

```plaintext
Input: s = "abccde"
Output: 3
Explanation: Longest substrings without any repeating characters are "abc" & "cde".
```

## Constraints

- `0 <= len(s) <= 5 * 10^4`
- `s` consists of English letters, digits, symbols and spaces

## Hints

<details>
<summary>Hint 1</summary>

A window has no repeats exactly when its number of distinct characters equals its length. Both are quantities you can track as the window moves.

</details>

<details>
<summary>Hint 2</summary>

When the character arriving on the right is already inside the window, one removal from the left may not be enough — the duplicate could be several positions in. Keep shrinking until it is gone.

</details>

## Solution

### Intuition

A window is valid when every character in it is unique. That is the same as saying the number of distinct characters equals the window length, which turns the validity test into a comparison between `len(counter)` and `window_end - window_start + 1`.

When a duplicate arrives, those two numbers diverge, so shrink from the left until they agree again. The `while` matters: the earlier copy of the duplicate may be well inside the window, and a single removal would not reach it.

```plaintext
s = "aabccbb"

a        {a:1}            1 distinct, length 1   valid    length 1
aa       {a:2}            1 distinct, length 2   repeat, shrink
 a       {a:1}            valid                  length 1
 ab      {a:1, b:1}       valid                  length 2
 abc     {a:1, b:1, c:1}  valid                  length 3   <- best
 abcc    {a:1, b:1, c:2}  3 distinct, length 4   repeat, shrink
  bcc    {b:1, c:2}       2 distinct, length 3   repeat, shrink
   cc    {c:2}            1 distinct, length 2   repeat, shrink
    c    {c:1}            valid                  length 1
    cb   {c:1, b:1}       valid                  length 2
    cbb  {c:1, b:2}       2 distinct, length 3   repeat, shrink
     bb  {b:2}            1 distinct, length 2   repeat, shrink
      b  {b:1}            valid                  length 1

answer = 3
```

### Algorithm

1. Add `s[window_end]` to the frequency map
2. While `len(counter)` is smaller than the window length, a duplicate is present: decrement the count of `s[window_start]`, remove the key if it reaches zero, and advance `window_start`
3. Record `window_end - window_start + 1` as a candidate for the longest length

### Complexity analysis

- Time complexity: $O(n)$ — each character is inserted once and removed at most once.
- Space complexity: $O(1)$ — see below.

#### Why is the space complexity $O(1)$?

Strictly, the map holds one key per distinct character in the window, so it is $O(\min(n, \Sigma))$ where $\Sigma$ is the alphabet size. In the worst case — a string with no repeats at all — every character ends up in the map, which looks like $O(n)$.

But the alphabet is fixed: $95 \text{ characters} = 52 \text{ (letters)} + 10 \text{ (digits)} + 32 \text{ (symbols)} + 1 \text{ (space)}$. The map can never exceed 95 entries no matter how long the input is, so the space is bounded by a constant. A fixed-size array of 95 slots would work just as well as a hash map here.

```python
from collections import Counter


class Solution:
    def length_of_longest_substring(self, s: str) -> int:
        longest_length = 0

        window_start = 0
        window_counter = Counter()
        for window_end in range(len(s)):
            window_counter[s[window_end]] += 1

            # fewer distinct characters than positions means something repeats
            while len(window_counter) < window_end - window_start + 1:
                start_character = s[window_start]
                window_counter[start_character] -= 1

                if window_counter[start_character] == 0:
                    del window_counter[start_character]

                window_start += 1

            longest_length = max(longest_length, window_end - window_start + 1)

        return longest_length
```
