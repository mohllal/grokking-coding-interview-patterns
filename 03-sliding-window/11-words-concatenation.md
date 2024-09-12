# Problem: Words Concatenation

LeetCode problem: [30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/).

Given a string and a list of words, find all the starting indices of substrings in the given string that are a concatenation of all the given words exactly once without any overlapping of words.

It is given that all words are of the same length.

## Examples

Example 1:

```plaintext
Input: String = "catfoxcat", Words = ["cat", "fox"]
Output: [0, 3]
Explanation: The two substring containing both the words are "catfox" & "foxcat".
```

Example 2:

```plaintext
Input: String = "catcatfoxfox", Words = ["cat", "fox"]
Output: [3]
Explanation: The only substring containing both the words is "catfox".
```

## Solution 

The algorithm iterates through the target string while keeping track of the frequency of words and matching them in chunks.

Here are the set of steps for our algorithm:

1. Keep the frequency of each word appears in the input list in a counter.
2. Slide a window across the string. For each possible starting index in the string (that could potentially match a valid substring), the window captures a portion of the string the size of all words combined. This is done by iterating through each chunk (of word length) within that window.
3. For each window, a counter keeps track of the words encountered so far in the window. It tries to match the words in the string by checking if each chunk of the current window corresponds to a valid word from the input list.
4. If a word is found that isn’t part of the list or if a word appears more times than it should (according to the frequency counter), the algorithm stops processing that window early since the window eventually will never be a valid substring and moves to the next possible starting index.
5. If all words in the current window are valid and appear in the right frequencies, the algorithm recognizes that it has found a valid starting point and stores that index.

Complexity analysis:

- Time complexity: O(N + M)
- Space complexity: O(N + M)

Where:

- `N` is length of the string `s`.
- `M` is length of the list `words`.
- `K` is length of the word.

```python
from collections import Counter

def findSubstring(s: str, words: List[str]) -> List[int]:
    indices = []

    words_count = len(words)
    word_length = len(words[0])
    total_length = words_count * word_length
    words_counter = Counter(words)
    
    # loop through the string `s`, where `i` is the starting index of the sliding window
    for i in range(len(s) - total_length + 1):
        window_counter = Counter()
        window_matched_words = 0

        # process the window in chunks of `word_length`, iterating through each word in the window
        for j in range(words_count):
            word_start = i + (j * word_length)
            word_end = word_start + word_length
            word = s[word_start:word_end]
            
            # if the word isn't in the list of words, break early
            if word not in words_counter:
                break
    
            window_counter[word] += 1
            
            # if the word count exceeds the count in the list words, break early
            if window_counter[word] > words_counter[word]:
                break
                
            # if the current word is valid and hasn't exceeded the count, increase matched words
            window_matched_words += 1
        
        if window_matched_words == words_count:
            indices.append(i)    

    return indices
```
