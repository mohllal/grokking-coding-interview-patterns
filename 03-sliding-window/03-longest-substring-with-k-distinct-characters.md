---
title: Longest Substring with K Distinct Characters
difficulty: Medium
leetcode_title: Longest Substring with At Most K Distinct Characters
leetcode: https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/
tags:
  - Hash Table
  - String
  - Sliding Window
---

# Longest Substring with K Distinct Characters

## Problem description

Given a string, find the length of the longest substring in it with no more than `k` distinct characters.

## Examples

**Example 1:**

```plaintext
Input: s = "araaci", k = 2
Output: 4
Explanation: The longest substring with no more than '2' distinct characters is "araa".
```

**Example 2:**

```plaintext
Input: s = "araaci", k = 1
Output: 2
Explanation: The longest substring with no more than '1' distinct characters is "aa".
```

**Example 3:**

```plaintext
Input: s = "cbbebi", k = 3
Output: 5
Explanation: The longest substrings with no more than '3' distinct characters are "cbbeb" & "bbebi".
```

## Constraints

- `1 <= len(s) <= 5 * 10^4`
- `0 <= k <= 50`

## Hints

<details>
<summary>Hint 1</summary>

You need to know how many *distinct* characters the window holds. A frequency map gives you that for free — but only if you keep it tidy.

</details>

<details>
<summary>Hint 2</summary>

When a character's count drops to zero it is no longer in the window. If you leave the key in the map, `len(map)` will overcount and you will shrink windows that were actually valid.

</details>

## Solution

### Intuition

Grow the window one character at a time. The window is valid while it holds at most `k` distinct characters, so the moment a new character pushes the count past `k`, shrink from the left until it is valid again — then measure.

The summary here is a frequency map, and `len(counter)` is the number of distinct characters. That equivalence only holds if keys are removed once their count reaches zero, which is the one bookkeeping detail this problem turns on.

```plaintext
s = "araaci",  k = 2

a        {a:1}              valid     length 1
ar       {a:1, r:1}         valid     length 2
ara      {a:2, r:1}         valid     length 3
araa     {a:3, r:1}         valid     length 4   <- best
araac    {a:3, r:1, c:1}    3 > 2, shrink from the left
 raac    {a:2, r:1, c:1}    still 3, shrink
  aac    {a:2, c:1}         valid     length 3
  aaci   {a:2, c:1, i:1}    3 > 2, shrink
   aci   {a:1, c:1, i:1}    still 3, shrink
    ci   {c:1, i:1}         valid     length 2

answer = 4
```

### Algorithm

1. Add `s[window_end]` to the frequency map
2. While the map holds more than `k` keys, decrement the count of `s[window_start]`, remove the key if it reaches zero, and advance `window_start`
3. Record `window_end - window_start + 1` as a candidate for the longest length

The measurement happens after the shrink loop, because only then is the window guaranteed valid.

### Complexity analysis

- Time complexity: $O(n)$ — each character is inserted once and removed at most once; `window_start` only moves forward.
- Space complexity: $O(k)$ — the map never holds more than `k + 1` keys, since it is trimmed back as soon as it exceeds `k`.

```python
from collections import Counter


class Solution:
    def length_of_longest_substring_k_distinct(self, s: str, k: int) -> int:
        longest_length = 0

        window_start = 0
        window_counter = Counter()
        for window_end in range(len(s)):
            window_counter[s[window_end]] += 1

            while len(window_counter) > k:
                start_character = s[window_start]
                window_counter[start_character] -= 1

                # drop exhausted keys, otherwise len() overcounts the distinct characters
                if window_counter[start_character] == 0:
                    del window_counter[start_character]

                window_start += 1

            longest_length = max(longest_length, window_end - window_start + 1)

        return longest_length
```
