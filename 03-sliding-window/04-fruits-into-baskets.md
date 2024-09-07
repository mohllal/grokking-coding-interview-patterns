# Problem: Fruits into Baskets

LeetCode problem: [904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/).

Given an array of characters where each character represents a fruit tree, you are given two baskets and your goal is to put maximum number of fruits in each basket. The only restriction is that each basket can have only one type of fruit.

You can start with any tree, but once you have started you can’t skip a tree. You will pick one fruit from each tree until you cannot, i.e., you will stop when you have to pick from a third fruit type.

Write a function to return the maximum number of fruits in both the baskets.

## Examples

Example 1:

```plaintext
Input: Fruit = ['A', 'B', 'C', 'A', 'C']
Output: 3
Explanation: We can put 2 'C' in one basket and one 'A' in the other from the subarray ['C', 'A', 'C']
```

Example 2:

```plaintext
Input: Fruit = ['A', 'B', 'C', 'B', 'B', 'C']
Output: 5
Explanation: We can put 3 'B' in one basket and two 'C' in the other basket. 
This can be done if we start with the second letter: ['B', 'C', 'B', 'B', 'C']
```

## Solution

This problem is quite similar to the [Longest Substring with K Distinct Characters](./03-longest-substring-with-k-distinct-characters.md) problem where `K` (representing the number of fruit types) is equal to `2`.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def totalFruit(fruits: List[int]) -> int:
    maximum_fruits = 0
    
    window_start = 0
    window_counter = Counter()
    for window_end in range(len(fruits)):
        window_counter[fruits[window_end]] += 1
        
        while len(window_counter) > 2:
            window_counter[fruits[window_start]] -= 1
            
            if window_counter[fruits[window_start]] == 0:
                window_counter.pop(fruits[window_start])
            
            window_start += 1
        
        maximum_fruits = max(maximum_fruits, (window_end - window_start) + 1)
    
    return maximum_fruits
```

## Similar Problem

LeetCode problem: [159. Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/).

Given a string, find the length of the longest substring in it with at most two distinct characters.

### Solution

This problem is exactly similar to the parent [Fruits into Baskets](#problem-fruits-into-baskets) problem.

Complexity analysis:

- Time complexity: O(N)
- Space complexity: O(1)

```python
def lengthOfLongestSubstringTwoDistinct(s: str) -> int:
    longest_length = 0
    
    window_start = 0
    window_counter = Counter()
    for window_end in range(len(s)):
        window_counter[s[window_end]] += 1
        
        while len(window_counter) > 2:
            window_counter[s[window_start]] -= 1
            
            if window_counter[s[window_start]] == 0:
                window_counter.pop(s[window_start])
            
            window_start += 1
        
        longest_length = max(longest_length, (window_end - window_start) + 1)
    
    return longest_length
```
