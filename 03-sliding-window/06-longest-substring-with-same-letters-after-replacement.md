---
title: Longest Substring with Same Letters after Replacement
difficulty: Medium
leetcode_title: Longest Repeating Character Replacement
leetcode: https://leetcode.com/problems/longest-repeating-character-replacement/
tags:
  - Hash Table
  - String
  - Sliding Window
---

# Longest Substring with Same Letters after Replacement

## Problem description

Given a string with lowercase letters only, if you are allowed to replace no more than `k` letters with any letter, find the length of the longest substring having the same letters after replacement.

## Examples

**Example 1:**

```plaintext
Input: s = "aabccbb", k = 2
Output: 5
Explanation: Replace the two 'c' with 'b' to have the longest repeating substring "bbbbb".
```

**Example 2:**

```plaintext
Input: s = "abbcb", k = 1
Output: 4
Explanation: Replace the 'c' with 'b' to have the longest repeating substring "bbbb".
```

**Example 3:**

```plaintext
Input: s = "abccde", k = 1
Output: 3
Explanation: Replace the 'b' or 'd' with 'c' to have the longest repeating substring "ccc".
```

## Constraints

- `1 <= len(s) <= 10^5`
- `s` consists of only uppercase or lowercase English letters
- `0 <= k <= len(s)`

## Hints

<details>
<summary>Hint 1</summary>

You never need to decide *which* letter to keep. Ask a cheaper question instead: for the current window, how many letters would you have to change to make them all identical?

</details>

<details>
<summary>Hint 2</summary>

Everything except the most frequent letter has to be replaced. So the cost of a window is `window_length - count_of_most_frequent_letter`, and the window is valid while that cost is at most `k`.

</details>

## Solution 1: Recompute the maximum

### Intuition

Trying each candidate letter in turn would be wasteful. The key reframing is that you never choose a letter at all: given any window, the cheapest way to make it uniform is to keep whichever letter already appears most often and replace everything else.

That gives a cost formula for the window:

```plaintext
replacements needed = window_length - count_of_most_frequent_letter
```

The window is valid while that number is at most `k`. Grow on every step, and shrink from the left whenever the cost exceeds `k`.

```plaintext
s = "aabccbb",  k = 2

a         {a:1}              len 1, max 1, cost 0    valid    length 1
aa        {a:2}              len 2, max 2, cost 0    valid    length 2
aab       {a:2,b:1}          len 3, max 2, cost 1    valid    length 3
aabc      {a:2,b:1,c:1}      len 4, max 2, cost 2    valid    length 4
aabcc     {a:2,b:1,c:2}      len 5, max 2, cost 3    3 > 2, shrink
 abcc     {a:1,b:1,c:2}      len 4, max 2, cost 2    valid    length 4
 abccb    {a:1,b:2,c:2}      len 5, max 2, cost 3    3 > 2, shrink
  bccb    {b:2,c:2}          len 4, max 2, cost 2    valid    length 4
  bccbb   {b:3,c:2}          len 5, max 3, cost 2    valid    length 5   <- best

answer = 5
```

### Algorithm

1. Add `s[window_end]` to the frequency map
2. While `window_length - max(counter.values()) > k`, decrement the count of `s[window_start]` and advance `window_start`
3. Record `window_end - window_start + 1` as a candidate for the longest length

### Complexity analysis

- Time complexity: $O(n)$ — the scan itself is linear, and each `max(counter.values())` call inspects at most 26 entries. That makes the work $O(26n)$, which is $O(n)$ once the constant is dropped.
- Space complexity: $O(1)$ — the map holds at most 26 lowercase letters, a constant independent of `n`.

```python
from collections import Counter


class Solution:
    def character_replacement(self, s: str, k: int) -> int:
        longest_length = 0

        window_start = 0
        window_counter = Counter()
        for window_end in range(len(s)):
            window_counter[s[window_end]] += 1

            # every letter except the most frequent one has to be replaced
            while (window_end - window_start + 1) - max(window_counter.values()) > k:
                window_counter[s[window_start]] -= 1
                window_start += 1

            longest_length = max(longest_length, window_end - window_start + 1)

        return longest_length
```

## Solution 2: Track the maximum

### Intuition

The `max(window_counter.values())` call is the only non-constant work left. It can be removed: track the highest frequency seen so far and update it only when a letter enters the window.

This makes `max_letter_count` an **overestimate** after a shrink — if the most frequent letter was the one removed, the stored value is now larger than any actual count in the window. That sounds like a bug, but it cannot produce a wrong answer.

Here is why. An overestimated `max_letter_count` makes the cost formula *understate* the true cost, so the window looks valid when it may not be — the loop stops shrinking too early. But `longest_length` only ever increases, and to record a **new** best the window must be strictly wider than the previous best. Growing wider while `max_letter_count` stays frozen raises the computed cost by one for every step, so the window cannot keep expanding on a stale value. To get a genuinely longer answer, some letter's count must actually reach a new high, which updates `max_letter_count` honestly.

In short: the overestimate can leave the window temporarily too wide, but it can never let a too-wide window be *recorded* as the answer.

### Algorithm

1. Add `s[window_end]` to the frequency map and update `max_letter_count`
2. While `window_length - max_letter_count > k`, decrement the count of `s[window_start]` and advance `window_start`, leaving `max_letter_count` untouched
3. Record `window_end - window_start + 1` as a candidate for the longest length

### Complexity analysis

- Time complexity: $O(n)$ — the same single pass, now with genuinely $O(1)$ work per step rather than $O(26)$.
- Space complexity: $O(1)$ — at most 26 letters in the map.

```python
from collections import Counter


class Solution:
    def character_replacement(self, s: str, k: int) -> int:
        longest_length = 0

        window_start = 0
        window_counter = Counter()
        max_letter_count = 0
        for window_end in range(len(s)):
            end_letter = s[window_end]
            window_counter[end_letter] += 1
            max_letter_count = max(max_letter_count, window_counter[end_letter])

            # max_letter_count is never lowered on shrink; see the reasoning above
            while (window_end - window_start + 1) - max_letter_count > k:
                window_counter[s[window_start]] -= 1
                window_start += 1

            longest_length = max(longest_length, window_end - window_start + 1)

        return longest_length
```
