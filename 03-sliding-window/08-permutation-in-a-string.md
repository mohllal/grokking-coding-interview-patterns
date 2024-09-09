
# Problem: Permutation in a String

LeetCode problem: [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/).

Given a pattern and a string, find out if the string contains any permutation of the pattern.

Permutation is defined as the re-arranging of the characters of the string. For example, `abc` has the following six permutations: `abc`, `acb`, `bac`, `bca`, `cab`, `cba`.

## Examples

Example 1:

```plaintext
Input: Pattern = "abc", String = "oidbcaf"
Output: true
Explanation: The string contains "bca" which is a permutation of the given pattern.
```

Example 2:

```plaintext
Input: Pattern = "dc", String = "odicf"
Output: false
Explanation: No permutation of the pattern is present in the given string as a substring.
```

Example 3:

```plaintext
Input: Pattern = "bcdyabcdx", String = "bcdxabcdy"
Output: true
Explanation: Both the string and the pattern are a permutation of each other.
```

Example 4:

```plaintext
Input: Pattern = "abc", String = "aaacb"
Output: true
Explanation: The string contains "acb" which is a permutation of the given pattern.
```

## Solution

The algorithm uses a sliding window of the same length as the input pattern. It maintains a frequency counter of the pattern's characters and compares this to the frequency of characters in the sliding window of the target string as the window moves over the string.

Here are the steps of the algorithm:

1. Create a counter to track the frequencies of all characters in the pattern string.
2. Iterate through the target string, adding one character at a time in the sliding window while maintaining a window of the same length as the pattern.
3. For each new character added to the window, decrease its frequency in the counter if it matches a character in the pattern. If its frequency reaches zero, that character is fully matched.
4. If at any time, the number of matched characters equals the number of unique characters in the pattern string, the window contains a permutation of the pattern.
5. When the window exceeds the pattern's length, the window is shrunk by removing the leftmost character, restoring its frequency in the counter if it belongs to the pattern and adjust the matched count accordingly.

Complexity analysis:

- Time complexity: O(N + M)
- Space complexity: O(N) or O(1) if `s1` and `s2` consist of lowercase English letters only.

Where:

- `N` is the length of `s1` (the pattern string).
- `M` is the length of `s2` (the target string).

```python
def checkInclusion(s1: str, s2: str) -> bool:
    # O(N) time and O(N) space
    s1_counter = Counter(s1)
    
    window_start = 0
    window_matched_characters = 0
    for window_end in range(len(s2)):
        window_end_character = s2[window_end]
        
        # if the character is part of s1, decrease its frequency in the counter
        if window_end_character in s1_counter:
            s1_counter[window_end_character] -= 1
            
            # if the frequency becomes zero, it means we have matched all instances of this character
            if s1_counter[window_end_character] == 0:
                window_matched_characters += 1
        
        # if all characters from s1 are matched, return True
        if window_matched_characters == len(s1_counter):
            return True

        # shrink the window when its length is longer than the pattern length
        if window_end >= len(s1) - 1:
            window_start_character = s2[window_start]
    
            # if the character that is leaving is part of s1, increment its frequency back in the counter
            if window_start_character in s1_counter:
                # if the character was previously fully matched, we lose the match for it
                if s1_counter[window_start_character] == 0:
                    window_matched_characters -= 1
                s1_counter[window_start_character] += 1
            
            window_start += 1
    
    return False
```
