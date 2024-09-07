# Problem: No-repeat Substring

LeetCode problem: [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/).

Given a string, find the length of the longest substring which has no repeating characters.

## Examples

Example 1:

```plaintext
Input: String = "aabccbb"
Output: 3
Explanation: The longest substring without any repeating characters is "abc".
```

Example 2:

```plaintext
Input: String = "abbbb"
Output: 2
Explanation: The longest substring without any repeating characters is "ab".
```

Example 3:

```plaintext
Input: String="abccde"
Output: 3
Explanation: Longest substrings without any repeating characters are "abc" & "cde".
```

## Solution

The algorithm implements a dynamic sliding window technique similar to the approach used in the [Longest Substring with K Distinct Characters](./03-longest-substring-with-k-distinct-characters.md) problem.

1. Incrementally slides the window by adding one character at a time to the end of the window.
2. If the number of distinct characters in the window is less than the current window length, it indicates the presence of duplicate characters. In this case, the algorithm shrinks the window from the start.
3. The window is shrunk until it no longer contains any duplicate characters, as the goal is to find the longest substring without duplicates.
4. During the shrinking process, the frequency of the character leaving the window is decremented, and if its count reaches `0`, it is removed from the counter.
5. After adjusting the window, the algorithm checks if the current window length is the longest so far and stores the result if true.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

Why space complexity is O(1)?

The space complexity of the algorithm will be O(K) where `K` is the number of distinct characters in the input string. This also means `K <= N`, because in the worst case, the whole string might not have any repeating character so the entire string will be added to the counter hashmap.

Having said that, since we can expect a fixed set of characters in the input string, $95 characters = 52 (letters) + 10 (digits) + 32 (symbols) + 1 (space)$, we can say that the algorithm runs in fixed space O(1); in this case, we can use a fixed-size array instead of the counter hashmap.

```python
from collections import Counter

def lengthOfLongestSubstring(s: str) -> int:
    longest_length = 0
    
    window_start = 0
    window_counter = Counter()
    for window_end in range(len(s)):
        window_counter[s[window_end]] += 1
        
        while len(window_counter) < (window_end - window_start) + 1:
            window_counter[s[window_start]] -= 1
            
            if window_counter[s[window_start]] == 0:
                window_counter.pop(s[window_start])
        
            window_start += 1
        
        longest_length = max(longest_length, (window_end - window_start) + 1)
        
    return longest_length
```
