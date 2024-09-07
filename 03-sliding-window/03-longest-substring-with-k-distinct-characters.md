# Problem: Longest Substring with K Distinct Characters

LeetCode problem: [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

Given a string, find the length of the longest substring in it with no more than `K` distinct characters.

## Examples

Example 1:

```plaintext
Input: String="araaci", K=2
Output: 4
Explanation: The longest substring with no more than '2' distinct characters is "araa".
```

Example 2:

```plaintext
Input: string = "araaci", K = 1
Output: 2
Explanation: The longest substring with no more than '1' distinct characters is "aa".
```

Example 3:

```plaintext
Input: string = "cbbebi", K = 3
Output: 5
Explanation: The longest substrings with no more than '3' distinct characters are "cbbeb" & "bbebi".
```

## Solution

The algorithm implements a dynamic sliding window technique similar to the approach used in the [Smallest Subarray with a Given Sum](./02-smallest-subarray-with-a-given-sum.md) problem. It uses a counter (or hashmap) to track the frequency of characters within the sliding window.

1. Incrementally slides the window by adding one character at a time to the end of the window.
2. If the number of distinct characters in the window exceeds `K`, the algorithm shrinks the window from the beginning.
3. The window is shrunk until it contains no more than `K` distinct characters, as the goal is to find the longest possible window with this constraint.
4. During the shrinking process, the frequency of the character leaving the window is decremented, and if its count reaches `0`, it is removed from the counter.
5. After adjusting the window, the algorithm checks if the current window length is the longest so far and stores the result if true.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(K)

Where:

- `N` is the length of the input string.
- `K` is the input number of distinct characters.

```python
from collections import Counter

def lengthOfLongestSubstringKDistinct(s: str, k: int) -> int:
    longest_length = 0

    window_start = 0
    window_counter = Counter()
    for window_end in range(len(s)):
        window_counter[s[window_end]] += 1
        
        while len(window_counter) > k:
            window_counter[s[window_start]] -= 1
            
            if window_counter[s[window_start]] == 0:
                window_counter.pop(s[window_start])
            
            window_start += 1
        
        longest_length = max(longest_length, (window_end - window_start) + 1)
    
    return longest_length
```
