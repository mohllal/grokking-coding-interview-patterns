# Problem: Longest Substring with Same Letters after Replacement

LeetCode problem: [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/).

Given a string with lowercase letters only, if you are allowed to replace no more than `K` letters with any letter, find the length of the longest substring having the same letters after replacement.

## Examples

Example 1:

```plaintext
Input: String = "aabccbb", k = 2
Output: 5
Explanation: Replace the two 'c' with 'b' to have a longest repeating substring "bbbbb".
```

Example 2:

```plaintext
Input: String = "abbcb", k = 1
Output: 4
Explanation: Replace the 'c' with 'b' to have a longest repeating substring "bbbb".
```

Example 3:

```plaintext
Input: String = "abccde", k = 1
Output: 3
Explanation: Replace the 'b' or 'd' with 'c' to have the longest repeating substring "ccc".
```

## Solution 1

The algorithm uses a dynamic-sized sliding window with greedy character replacement.

1. The algorithm maintains a dynamic-sized window that expands by moving the end of the window to the right.
2. As the window expands, it tracks the frequency of characters within the window to determine the most frequent character.
3. The number of replacements needed is calculated by: `(window size) - (frequency of the most frequent character)`. This gives the number of characters that must be changed to make the substring consists of one character.
4. If the number of character replacements exceeds `K`, the window is shrunk by moving the start of the window to the right until the number of replacements become less than `K`.
5. Throughout this process, the algorithm keeps track of the maximum length of a valid substring (one where the number of replacements is within the allowed limit).

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

Why time complexity is O(N) and space complexity is O(1)?

The algorithm processes each character in the input string at most twice: once when expanding the window and once when shrinking it.

1. The outer loop that expands the window runs in O(N) time, where `N` is the number of characters in the input string.
2. The inner loop uses the counter hashmap to finds the maximum frequency in each iteration. Since the input string contains only lowercase letters, the maximum number of unique characters is `26`. Hence, this operation is effectively O(26), which is a constant time operation.

Combining these, the overall complexity is O((26 * N) + N), which simplifies to O(N) when dropping the constant factor.

Similarly, the space complexity is O(1) because the maximum number of unique characters in the counter hashmap is 26, which is a constant.

```python
from collections import Counter

def characterReplacement1(s: str, k: int) -> int:
    longest_length = 0
    
    window_start = 0
    window_counter = Counter()
    for window_end in range(len(s)):
        window_end_letter = s[window_end]        
        window_counter[window_end_letter] += 1

        # number of replacements = (window size) - (frequency of the most frequent character)
        # if replacements exceed k, shrink the window by moving the start
        while (window_end - window_start + 1) - max(window_counter.values()) > k:
            window_start_letter = s[window_start]
            window_counter[window_start_letter] -= 1
            window_start += 1
        
        longest_length = max(longest_length, (window_end - window_start) + 1)
        
    return longest_length
```

## Solution 2

The improved algorithm eliminates the need to repeatedly search for the most frequent character in the window during each iteration in the shrinking inner loop. Instead, it tracks the maximum frequency of any character (`max_letter_count`) as the window expands, avoiding recalculation when the window shrinks.

The idea is that `max_letter_count` might become an overestimation when the window shrinks (e.g., if the most frequent character is removed from the window). However, this overestimation doesn’t affect the correctness of the algorithm. As long as the window remains valid, the overestimated `max_letter_count` still ensures the algorithm correctly identifies the longest possible substring that satisfies the replacement condition.

In other words, the `max_letter_count` overestimation does not cause the algorithm to miss a longer valid window. It just means the algorithm might temporarily shrink a window unnecessarily.

For example:

1. If the current window size is `4` and `max_letter_count` is `3`, the window is valid as long as `(window size - max_letter_count) ≤ k`.
2. When the window shrinks, the actual maximum frequency might decrease, but the algorithm only cares about expanding the window to find longer valid substrings.
3. If a longer window is found, a higher maximum frequency (`max_letter_count`) will be needed to maintain the validity of the window under the replacement limit e.g. for longer window size `5`, the `max_letter_count` must be equal to `4` to maintain the same replacement limit `K`.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
from collections import Counter

def characterReplacement(s: str, k: int) -> int:
    longest_length = 0
    
    window_start = 0
    window_counter = Counter()
    max_letter_count = 0
    for window_end in range(len(s)):
        window_end_letter = s[window_end]
        window_counter[window_end_letter] += 1

        max_letter_count = max(max_letter_count, window_counter[window_end_letter])

        # if the number of replacements needed exceeds k, shrink the window
        while (window_end - window_start + 1) - max_letter_count > k:
            window_start_letter = s[window_start]
            window_counter[window_start_letter] -= 1
            window_start += 1
        
        longest_length = max(longest_length, (window_end - window_start) + 1)
        
    return longest_length
```
