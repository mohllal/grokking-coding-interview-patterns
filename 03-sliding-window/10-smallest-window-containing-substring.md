---
title: Smallest Window Containing Substring
difficulty: Hard
leetcode_title: Minimum Window Substring
leetcode: https://leetcode.com/problems/minimum-window-substring/
tags:
  - Hash Table
  - String
  - Sliding Window
---

# Smallest Window Containing Substring

## Problem description

Given a string and a pattern, find the smallest substring in the given string which has all the characters of the given pattern.

If no such substring exists, return the empty string.

## Examples

**Example 1:**

```plaintext
Input: s = "aabdec", pattern = "abc"
Output: "abdec"
Explanation: The smallest substring having all characters of the pattern is "abdec".
```

**Example 2:**

```plaintext
Input: s = "abdabca", pattern = "abc"
Output: "abc"
Explanation: The smallest substring having all characters of the pattern is "abc".
```

**Example 3:**

```plaintext
Input: s = "adcad", pattern = "abc"
Output: ""
Explanation: No substring in the given string has all characters of the pattern.
```

## Constraints

- `1 <= len(s), len(p) <= 10^5`
- `s` and `p` consist of uppercase and lowercase English letters
- Characters may repeat in the pattern, and every repetition must be covered

## Hints

<details>
<summary>Hint 1</summary>

The window is no longer a fixed width — extra characters between the ones you need are allowed. So what tells you when to stop growing, and when to start shrinking?

</details>

<details>
<summary>Hint 2</summary>

Once the window contains everything, shrink from the left while it still contains everything. The moment a required character is about to leave, that window is as small as it gets for this right boundary.

</details>

## Solution

### Intuition

This shares the `matched` counter with [Permutation in a String](./08-permutation-in-a-string.md), but the window is variable rather than fixed: the answer may contain junk characters in between, as long as every required character is present.

That makes it a **shortest valid window** problem. Grow until the window becomes valid — every pattern character covered — then shrink from the left for as long as it stays valid, recording the smallest length seen. As soon as removing a character would break coverage, stop shrinking and grow again.

```plaintext
pattern = "abc"  ->  need {a:1, b:1, c:1}
s = "abdabca"

end=0   a          matched 1
end=1   ab         matched 2
end=2   abd        matched 2
end=3   abda       matched 2   ('a' surplus)
end=4   abdab      matched 2   ('b' surplus)
end=5   abdabc     matched 3   valid -> record "abdabc" (6), drop 'a'
         bdabc     matched 3   still valid -> record "bdabc" (5), drop 'b'
          dabc     matched 3   still valid -> record "dabc" (4), drop 'd'
           abc     matched 3   still valid -> record "abc" (3), drop 'a' breaks it
end=6       bca    matched 3   valid -> length 3, no improvement, drop 'b'

answer = "abc"
```

### Algorithm

1. Build a frequency map of the pattern
2. For each character entering on the right, if it belongs to the pattern, decrement its count; if the count becomes `0`, increment `matched`
3. While `matched` equals the number of distinct pattern characters, the window is valid: record its start and length if it is the shortest so far, then remove `s[window_start]`, restoring its count and decrementing `matched` if that character was exactly satisfied
4. Slice the best window out at the end

Recording happens before the removal, because the current window is the candidate. This is the opposite of a longest-window problem, where the answer is recorded only after validity has been restored.

### Complexity analysis

- Time complexity: $O(n + m)$ — $O(n)$ to build the pattern's map, then a single pass in which each index enters and leaves the window at most once. Tracking the best window by index rather than by slicing keeps the inner loop $O(1)$ per step.
- Space complexity: $O(n)$ for the pattern's frequency map, which is $O(1)$ for a fixed alphabet. The returned substring is not counted.

Where `n` is the length of the pattern and `m` is the length of the string being searched.

```python
from collections import Counter


class Solution:
    def min_window(self, s: str, p: str) -> str:
        pattern_counter = Counter(p)
        distinct_needed = len(pattern_counter)

        minimum_length = float("inf")
        minimum_start = 0

        window_start = 0
        matched = 0
        for window_end in range(len(s)):
            end_character = s[window_end]

            if end_character in pattern_counter:
                pattern_counter[end_character] -= 1

                if pattern_counter[end_character] == 0:
                    matched += 1

            # shrink while the window still covers the whole pattern
            while matched == distinct_needed:
                if window_end - window_start + 1 < minimum_length:
                    minimum_length = window_end - window_start + 1
                    minimum_start = window_start

                start_character = s[window_start]
                if start_character in pattern_counter:
                    # removing it drops coverage, so the window stops being valid
                    if pattern_counter[start_character] == 0:
                        matched -= 1

                    pattern_counter[start_character] += 1

                window_start += 1

        if minimum_length == float("inf"):
            return ""

        return s[minimum_start:minimum_start + minimum_length]
```

Tracking `minimum_start` and `minimum_length` rather than slicing a candidate string on every shrink matters for the complexity: slicing inside the loop would copy up to `m` characters each time, degrading the scan to $O(m^2)$ on inputs where the window stays valid for long stretches.
