# Problem: Smallest Window Containing Substring

LeetCode problem: [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/).

Given a string and a pattern, find the smallest substring in the given string which has all the characters of the given pattern.

## Examples

Example 1:

```plaintext
Input: String = "aabdec", Pattern = "abc"
Output: "abdec"
Explanation: The smallest substring having all characters of the pattern is "abdec"
```

Example 2:

```plaintext
Input: String = "abdabca", Pattern = "abc"
Output: "abc"
Explanation: The smallest substring having all characters of the pattern is "abc".
```

Example 3:

```plaintext
Input: String = "adcad", Pattern = "abc"
Output: ""
Explanation: No substring in the given string has all characters of the pattern.
```

## Solution

This problem is quite similar to the [Permutation in a String](./08-permutation-in-a-string.md) problem, except that here we are only concerned with finding a substring that contains all the characters of the pattern, possibly with additional characters in between.

The algorithm uses a dynamic-sized sliding window to scan through the target string. The window expands by moving the right pointer until all characters of the pattern are included (or not).

Once a valid window is found, all of the pattern's characters are matched, the window is shrunk from the beginning to find the shortest possible substring that still contains all the characters of the pattern. The shrinking process stops as soon as we remove a matched character from the sliding window, at which point the window expands again. The smallest valid window found during this process is returned as the result.

Complexity analysis:

- Time complexity: O(N + M)
- Space complexity: O(N + M)

Where:

- `N` is the length of `p` (the pattern string).
- `M` is the length of `s` (the target string).

```python
def minWindow(s: str, p: str) -> str:
    result = ""
    p_counter = Counter(p)
    
    window_start = 0
    window_matched_characters = 0
    for window_end in range(len(s)):
        window_end_character = s[window_end]
        
        if window_end_character in p_counter:
            p_counter[window_end_character] -= 1

            if p_counter[window_end_character] == 0:
                window_matched_characters += 1
        
        while window_matched_characters == len(p_counter):
            candidate = s[window_start:window_end+1]
            if result == "" or len(candidate) < len(result):
                result = candidate
            
            window_start_character = s[window_start]
            if window_start_character in p_counter:
                if p_counter[window_start_character] == 0:
                    window_matched_characters -= 1

                p_counter[window_start_character] += 1
            
            window_start += 1
    
    return result
```
