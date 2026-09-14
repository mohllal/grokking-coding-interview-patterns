---
title: String Anagrams
difficulty: Medium
leetcode_title: Find All Anagrams in a String
leetcode: https://leetcode.com/problems/find-all-anagrams-in-a-string/
tags:
  - Hash Table
  - String
  - Sliding Window
---

# String Anagrams

## Problem description

Given a string and a pattern, find all anagrams of the pattern in the given string. Return a list of the starting indices of the anagrams.

An anagram is a permutation of a string. For example, `abc` has six anagrams: `abc`, `acb`, `bac`, `bca`, `cab`, `cba`.

## Examples

**Example 1:**

```plaintext
Input: s = "ppqp", pattern = "pq"
Output: [1, 2]
Explanation: The two anagrams of the pattern in the given string are "pq" and "qp".
```

**Example 2:**

```plaintext
Input: s = "abbcabc", pattern = "abc"
Output: [2, 3, 4]
Explanation: The three anagrams of the pattern in the given string are "bca", "cab", and "abc".
```

## Constraints

- `1 <= len(s), len(p) <= 3 * 10^4`
- `s` and `p` consist of lowercase English letters

## Hints

<details>
<summary>Hint 1</summary>

Every anagram has exactly the pattern's length, so the window size never changes. What varies is only whether the letter counts line up.

</details>

<details>
<summary>Hint 2</summary>

Unlike a "does one exist?" question, you cannot stop at the first hit. What do you record, and at which moment is `window_start` the right index to record?

</details>

## Solution

### Intuition

An anagram of the pattern is a substring with the same length and the same letter counts — exactly the condition from [Permutation in a String](./08-permutation-in-a-string.md). Slide a window of the pattern's width and keep a `matched` counter of how many distinct letters currently have precisely the right count.

The difference is what to do on a match. Instead of returning early, record `window_start` and carry on, because every later anagram must also be found.

```plaintext
pattern = "abc"  ->  need {a:1, b:1, c:1},  window size 3
s = "abbcabc"

end=0   a       add 'a' -> matched 1
end=1   ab      add 'b' -> matched 2
end=2   abb     add 'b' -> surplus, matched 2     drop 'a' -> matched 1
end=3    bbc    add 'c' -> matched 2              drop 'b' -> surplus absorbed, matched 2
end=4     bca   add 'a' -> matched 3   record 2   drop 'b' -> matched 2
end=5      cab  add 'b' -> matched 3   record 3   drop 'c' -> matched 2
end=6       abc add 'c' -> matched 3   record 4

answer = [2, 3, 4]
```

Note the asymmetry in how `matched` is maintained. A letter's count is allowed to go negative when the window holds more copies than the pattern needs, and `matched` is left alone — the requirement is still met, just over-met. On the way out, `matched` drops only when the count is exactly `0`, meaning the window genuinely depended on the departing copy. Removing a surplus copy merely takes the count from `-1` back to `0`, leaving the letter satisfied.

### Algorithm

1. Build a frequency map of the pattern
2. For each character entering on the right, if it belongs to the pattern, decrement its count; if the count becomes `0`, increment `matched`
3. If `matched` equals the number of distinct characters in the pattern, append `window_start` to the results
4. Once the window exceeds the pattern's length, remove `s[window_start]`: if it belongs to the pattern and its count is currently `0`, that match is lost, so decrement `matched`; then restore its count and advance `window_start`

### Complexity analysis

- Time complexity: $O(n + m)$ — $O(n)$ to build the pattern's map, then one pass over the string with $O(1)$ work per index.
- Space complexity: $O(n)$ for the pattern's frequency map, which is $O(1)$ when the input is restricted to lowercase English letters. The output list is not counted.

Where `n` is the length of the pattern and `m` is the length of the string being searched.

```python
from collections import Counter
from typing import List


class Solution:
    def find_anagrams(self, s: str, p: str) -> List[int]:
        anagrams = []

        pattern_counter = Counter(p)
        distinct_needed = len(pattern_counter)

        window_start = 0
        matched = 0
        for window_end in range(len(s)):
            end_character = s[window_end]

            if end_character in pattern_counter:
                pattern_counter[end_character] -= 1

                # reaching zero means this character is now fully satisfied
                if pattern_counter[end_character] == 0:
                    matched += 1

            if matched == distinct_needed:
                anagrams.append(window_start)

            # keep the window exactly len(p) wide
            if window_end >= len(p) - 1:
                start_character = s[window_start]

                if start_character in pattern_counter:
                    # it was satisfied, and it is about to leave
                    if pattern_counter[start_character] == 0:
                        matched -= 1

                    pattern_counter[start_character] += 1

                window_start += 1

        return anagrams
```

`matched` can only reach `distinct_needed` once the window already holds at least `len(p)` characters, so `window_start` is always the true start of a full-width window at the moment it is recorded.
