---
title: Words Concatenation
difficulty: Hard
leetcode_title: Substring with Concatenation of All Words
leetcode: https://leetcode.com/problems/substring-with-concatenation-of-all-words/
tags:
  - Hash Table
  - String
  - Sliding Window
---

# Words Concatenation

## Problem description

Given a string and a list of words, find all the starting indices of substrings in the given string that are a concatenation of all the given words exactly once, without any overlapping of words.

All words are of the same length.

## Examples

**Example 1:**

```plaintext
Input: s = "catfoxcat", words = ["cat", "fox"]
Output: [0, 3]
Explanation: The two substrings containing both words are "catfox" and "foxcat".
```

**Example 2:**

```plaintext
Input: s = "catcatfoxfox", words = ["cat", "fox"]
Output: [3]
Explanation: The only substring containing both words is "catfox".
```

## Constraints

- `1 <= len(s) <= 10^4`
- `1 <= len(words) <= 5000`
- `1 <= len(words[i]) <= 30`
- All words have the same length
- `s` and `words[i]` consist of lowercase English letters

## Hints

<details>
<summary>Hint 1</summary>

Because every word has the same length, a valid substring has a known total length, and it splits into word-sized chunks at fixed offsets. You never have to consider partial words.

</details>

<details>
<summary>Hint 2</summary>

Within one candidate, you can stop early: the moment you see a chunk that is not a word, or a word appearing more times than the list allows, no longer prefix from this start can work.

</details>

## Solution

### Intuition

The fixed word length is what makes this tractable. A valid substring is always `len(words) * len(word)` characters long, and it divides cleanly into `len(words)` chunks. So the window slides one character at a time, but its *contents* are read in word-sized steps rather than character-sized ones.

For each start index, walk the chunks and count them against the required multiset. Two conditions kill a candidate immediately: a chunk that is not a word at all, or a word seen more often than the list provides.

```plaintext
s = "catcatfoxfox",  words = ["cat", "fox"]  ->  need {cat:1, fox:1}
total length = 2 * 3 = 6

i=0   | cat | cat |    'cat' seen twice, need 1   -> reject
i=1   | atc | atf |    'atc' is not a word        -> reject
i=2   | tca | tfo |    'tca' is not a word        -> reject
i=3   | cat | fox |    both satisfied             -> accept, record 3
i=4   | atf | oxf |    'atf' is not a word        -> reject
i=5   | tfo | xfo |    'tfo' is not a word        -> reject
i=6   | fox | fox |    'fox' seen twice, need 1   -> reject

answer = [3]
```

### Algorithm

1. Count the required words into a frequency map
2. For every start index `i` where a full concatenation could still fit
3. Read the window as `len(words)` consecutive chunks of `word_length`
4. Reject the start index as soon as a chunk is not a required word, or its count in the window exceeds the requirement
5. If all chunks are consumed without rejection, record `i`

### Complexity analysis

- Time complexity: $O(n \cdot m \cdot k)$ — there are roughly $n$ start positions, each examines up to $m$ chunks, and slicing each chunk copies $k$ characters.
- Space complexity: $O(m \cdot k)$ — the two frequency maps hold up to $m$ words of $k$ characters each.

Where `n` is the length of `s`, `m` is the number of words, and `k` is the length of each word.

```python
from collections import Counter
from typing import List


class Solution:
    def find_substring(self, s: str, words: List[str]) -> List[int]:
        indices = []

        words_count = len(words)
        word_length = len(words[0])
        total_length = words_count * word_length
        words_counter = Counter(words)

        for start in range(len(s) - total_length + 1):
            window_counter = Counter()
            matched_words = 0

            # read the candidate as consecutive word-sized chunks
            for chunk in range(words_count):
                chunk_start = start + chunk * word_length
                word = s[chunk_start:chunk_start + word_length]

                if word not in words_counter:
                    break

                window_counter[word] += 1

                # too many copies of this word, so no longer prefix can work
                if window_counter[word] > words_counter[word]:
                    break

                matched_words += 1

            if matched_words == words_count:
                indices.append(start)

        return indices
```

The early `break` on a surplus word is what keeps this practical: without it, every start index would always cost a full `m` chunks.
