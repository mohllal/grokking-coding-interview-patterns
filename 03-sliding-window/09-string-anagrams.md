# Problem: String Anagrams

LeetCode problem: [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/).

Given a string and a pattern, find all anagrams of the pattern in the given string.

Anagram is actually a Permutation of a string. For example, `abc` has the following six anagrams: `abc`, `acb`, `bac`, `bca`, `cab`, `cba`.

## Examples

Example 1:

```plaintext
Input: String = "ppqp", Pattern = "pq"
Output: [1, 2]
Explanation: The two anagrams of the pattern in the given string are "pq" and "qp".
```

Example 2:

```plaintext
Input: String = "abbcabc", Pattern = "abc"
Output: [2, 3, 4]
Explanation: The three anagrams of the pattern in the given string are "bca", "cab", and "abc".
```

## Solution

This problem is exactly the same as the [Permutation in a String](./08-permutation-in-a-string.md) problem except that instead of returning a boolean result indicating whether a valid permutation exists, we will return the starting indices of all valid windows where the permutation is found.

Complexity analysis:

- Time complexity: O(N + M)
- Space complexity: O(N) or O(1) if `s` and `p` consist of lowercase English letters only.

Where:

- `N` is the length of `p` (the pattern string).
- `M` is the length of `s` (the target string).

```python
def findAnagrams(s: str, p: str) -> List[int]:
    anagrams = []

    # O(N) time and O(N) space
    p_counter = Counter(p)

    window_start = 0
    window_matched_characters = 0
    for window_end in range(len(s)):
        window_end_character = s[window_end]
        
        # if the letter is part of p, decrease its frequency in the counter
        if window_end_character in p_counter:
            p_counter[window_end_character] -= 1

            # if the frequency becomes zero, it means we have matched all instances of this letter
            if p_counter[window_end_character] == 0:
                window_matched_characters += 1
        
        # if all letters from p are matched, save window start
        if window_matched_characters == len(p_counter):
            anagrams.append(window_start)
        
        # shrink the window when its length is longer than the pattern length
        if window_end >= len(p) - 1:
            window_start_character = s[window_start]
            
            # if the letter that is leaving is part of s1, increment its frequency back in the counter
            if window_start_character in p_counter:
                # if the letter was previously fully matched, we lose the match for it
                if p_counter[window_start_character] == 0:
                    window_matched_characters -= 1

                p_counter[window_start_character] += 1

            window_start += 1

    return anagrams
```
