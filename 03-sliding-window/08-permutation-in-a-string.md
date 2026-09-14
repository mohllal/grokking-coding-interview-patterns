---
title: Permutation in a String
difficulty: Medium
leetcode_title: Permutation in String
leetcode: https://leetcode.com/problems/permutation-in-string/
tags:
  - Hash Table
  - Two Pointers
  - String
  - Sliding Window
---

# Permutation in a String

## Problem description

Given a pattern and a string, find out if the string contains any permutation of the pattern.

A permutation is a re-arrangement of the characters of a string. For example, `abc` has six permutations: `abc`, `acb`, `bac`, `bca`, `cab`, `cba`.

## Examples

**Example 1:**

```plaintext
Input: pattern = "abc", s = "oidbcaf"
Output: true
Explanation: The string contains "bca" which is a permutation of the given pattern.
```

**Example 2:**

```plaintext
Input: pattern = "dc", s = "odicf"
Output: false
Explanation: No permutation of the pattern is present in the given string as a substring.
```

**Example 3:**

```plaintext
Input: pattern = "bcdyabcdx", s = "bcdxabcdy"
Output: true
Explanation: Both the string and the pattern are a permutation of each other.
```

**Example 4:**

```plaintext
Input: pattern = "abc", s = "aaacb"
Output: true
Explanation: The string contains "acb" which is a permutation of the given pattern.
```

## Constraints

- `1 <= len(s1), len(s2) <= 10^4`
- `s1` and `s2` consist of lowercase English letters

## Hints

<details>
<summary>Hint 1</summary>

A permutation of the pattern has exactly the pattern's length and exactly the pattern's letter counts. That fixes the window size for you.

</details>

<details>
<summary>Hint 2</summary>

Comparing two frequency maps at every index is wasteful. Keep one number instead: how many distinct characters currently have exactly the right count.

</details>

## Solution

### Intuition

A permutation of the pattern is a substring of the same length with the same multiset of characters. The length is fixed, so this is a fixed-size window — the only question is how to test the match cheaply.

Comparing the full window map against the pattern map at each step would cost $O(\Sigma)$ per index. Instead, keep a single `matched` counter: decrement a character's outstanding requirement as it enters, and when that requirement hits exactly zero, one more distinct character is fully satisfied. When `matched` equals the number of distinct characters in the pattern, the window is a permutation.

```plaintext
pattern = "abc"  ->  need {a:1, b:1, c:1},  window size 3
s = "oidbcaf"

end=0   o        not needed                           matched 0
end=1   oi       not needed                           matched 0
end=2   oid      not needed                           matched 0   window full, drop 'o'
end=3    idb     'b' satisfied                        matched 1   drop 'i'
end=4     dbc    'c' satisfied                        matched 2   drop 'd'
end=5      bca   'a' satisfied                        matched 3   <- all three matched

answer = true
```

A character's count is allowed to go negative when the window holds more copies than the pattern needs. `matched` is deliberately left alone in that case — the requirement is still met, just over-met. On the way out, `matched` drops only when the count is exactly `0`, meaning the window genuinely depended on the departing copy; removing a surplus copy merely takes the count from `-1` back to `0`.

### Algorithm

1. Build a frequency map of the pattern
2. For each character entering on the right, if it belongs to the pattern, decrement its count; if the count becomes `0`, increment `matched`
3. If `matched` equals the number of distinct characters in the pattern, a permutation has been found
4. Once the window exceeds the pattern's length, remove `s[window_start]`: if it belongs to the pattern and its count is currently `0`, that match is lost, so decrement `matched`; then restore its count and advance `window_start`

### Complexity analysis

- Time complexity: $O(n + m)$ — $O(n)$ to build the pattern's map, then a single pass over the string. The match test is $O(1)$, not a map comparison.
- Space complexity: $O(n)$ for the pattern's frequency map, which is $O(1)$ when the input is restricted to lowercase English letters.

Where `n` is the length of the pattern and `m` is the length of the string being searched.

```python
from collections import Counter


class Solution:
    def check_inclusion(self, s1: str, s2: str) -> bool:
        pattern_counter = Counter(s1)
        distinct_needed = len(pattern_counter)

        window_start = 0
        matched = 0
        for window_end in range(len(s2)):
            end_character = s2[window_end]

            if end_character in pattern_counter:
                pattern_counter[end_character] -= 1

                # reaching zero means this character is now fully satisfied
                if pattern_counter[end_character] == 0:
                    matched += 1

            if matched == distinct_needed:
                return True

            # keep the window exactly len(s1) wide
            if window_end >= len(s1) - 1:
                start_character = s2[window_start]

                if start_character in pattern_counter:
                    # it was satisfied, and it is about to leave
                    if pattern_counter[start_character] == 0:
                        matched -= 1

                    pattern_counter[start_character] += 1

                window_start += 1

        return False
```

## Relationship to [String Anagrams](./09-string-anagrams.md)

Identical machinery — same fixed window, same `matched` counter, same shrink logic. The only difference is what happens on a hit: this problem returns `True` immediately, while String Anagrams records the window's start index and keeps scanning for the rest.
