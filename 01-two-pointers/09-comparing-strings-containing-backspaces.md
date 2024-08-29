# Problem: Comparing Strings Containing Backspaces

LeetCode problem: [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/).

Given two strings containing backspaces (identified by the character `#`), check if the two strings are equal.

## Examples

Example 1:

```plaintext
Input: str1 = "xy#z", str2 = "xzz#"
Output: true
Explanation: After applying backspaces the strings become "xz" and "xz" respectively.
```

Example 2:

```plaintext
Input: str1 = "xy#z", str2 = "xyz#"
Output: false
Explanation: After applying backspaces the strings become "xz" and "xy" respectively.
```

Example 3:

```plaintext
Input: str1 = "xp#", str2 = "xyz##"
Output: true
Explanation: After applying backspaces the strings become "x" and "x" respectively.
In "xyz##", the first '#' removes the character 'z' and the second '#' removes the character 'y'.
```

Example 4:

```plaintext
Input: str1 = "xywrrmp", str2 = "xywrrmu#p"
Output: true
Explanation: After applying backspaces the strings become "xywrrmp" and "xywrrmp" respectively.
```

## Solution 1

The algorithm builds both strings by skipping characters affected by backspaces (`#`). It then compares the two final strings, returning true if they are identical, or false if they differ.

Complexity analysis:

- Time complexity: O(N+M)
- Space complexity: O(N+M)

Where `N` and `M` are the lengths of the two input strings respectively.

```python
# O(K) time and O(K) space
# where K is the length of the string parameter
def removeBackspaces(string: str) -> str:
    string_new = ""
    string_backspaces = 0

    current = len(string) - 1
    while current >= 0:
        if string[current] == "#":
            string_backspaces += 1
        elif string_backspaces > 0:
            string_backspaces -= 1
        else:
            string_new += string[current]

        current -= 1
    
    return string_new[::-1]

def backspaceCompare(s: str, t: str) -> bool: 
    s_new = removeBackspaces(s)
    t_new = removeBackspaces(t)

    return s_new == t_new
```

## Solution 2

The algorithm uses two pointers to traverse the strings from the end to the beginning, skipping characters that are deleted by backspaces (`#`).

To handle the backspaces, it finds the next valid character in each string by counting the backspaces (`#`) and skipping over the characters that would be deleted. Once both pointers are positioned at valid characters, those characters are compared. If they match, the pointers move to the previous valid characters and repeat the process, otherwise it returns false.

If at any point, one string still has valid characters remaining while the other does not, it returns false.

Complexity analysis:

- Time complexity: O(N+M)
- Space complexity: O(1)

Where `N` and `M` are the lengths of the two input strings respectively.

```python
# O(K) time and O(1) space
# where K is the length of the string parameter
def getNextValidIndex(string: str, index: int) -> int:
    backspaces = 0
    while index >= 0:
        if string[index] == "#":
            backspaces += 1
        elif backspaces > 0:
            backspaces -= 1 # found a character that is affected by a backspace, skip it
        else:
            break # found a valid character that is not affected by any backspace
        index -= 1

    return index

def backspaceCompare(s: str, t: str) -> bool:
    current_s = len(s) - 1
    current_t = len(t) - 1
    while current_s >= 0 or current_t >= 0:
        current_s = getNextValidIndex(s, current_s)
        current_t = getNextValidIndex(t, current_t)
    
        # if both pointers have valid characters, compare them
        if current_s >= 0 and current_t >= 0 and s[current_s] != t[current_t]:
            return False
        
        # if one string has a valid character left and the other doesn't,
        if (current_s >= 0 and current_t < 0) or (current_t >= 0 and current_s < 0):
            return False
        
        current_s -= 1
        current_t -= 1
    
    return True
```
