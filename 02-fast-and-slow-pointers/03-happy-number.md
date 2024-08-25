# Problem: Happy Number

LeetCode problem: [202. Happy Number](https://leetcode.com/problems/happy-number/).

Any number will be called a happy number if, after repeatedly replacing it with a number equal to the sum of the square of all of its digits, leads us to number `1`.

All other (not-happy) numbers will never reach `1`. Instead, they will be stuck in a cycle of numbers which does not include `1`.

## Examples

Example 1:

```plaintext
Input: 23   
Output: True (23 is a happy number)
```

Example 2:

```plaintext
Input: 12   
Output: False (12 is not a happy number) 
```

## Solution 1

The process, defined above, to find out if a number is a happy number or not, always ends in a cycle. If the number is a happy number, the process will be stuck in a cycle on number `1`, and if the number is not a happy number then the process will be stuck in a cycle with a set of numbers.

Example of un-happy number `12`

```plaintext
Input: 12

Steps:
1.  (1^2) + (2^2) = 1 + 4 = 5
2.  (5^2) = 25
3.  (2^2) + (5^2) = 4 + 25 = 29
4.  (2^2) + (9^2) = 4 + 81 = 85
5.  (8^2) + (5^2) = 64 + 25 = 89
6.  (8^2) + (9^2) = 64 + 81 = 145
7.  (1^2) + (4^2) + (5^2) = 1 + 16 + 25 = 42
8.  (4^2) + (2^2) = 16 + 4 = 20
9.  (2^2) + (0^2) = 4 + 0 = 4
10. (4^2) = 16
11. (1^2) + (6^2) = 1 + 36 = 37
12. (3^2) + (7^2) = 9 + 49 = 58
13. (5^2) + (8^2) = 25 + 64 = 89 (cycle to step #5)

Linked list representation:
12 ──▶ 5 ──▶ 25 ──▶ 29 ──▶ 85 ──▶ 89 ──▶ ... ──▶ 37 ──▶ 58
                                  ▲                     │
                                  │                     │
                                  │                     ▼
                                  └─────────────────── (cycle)
Output: False
```

Example of happy number `49`

```plaintext
Input: 49

Steps:
1. (4^2) + (9^2) = 16 + 81 = 97
2. (9^2) + (7^2) = 81 + 49 = 130
3. (1^2) + (3^2) + (0^2) = 1 + 9 + 0 = 10
4. (1^2) + (0^2) = 1 + 0 = 1 (cycle)

Linked list representation:
40 ──▶ 97 ──▶ 130 ──▶ 10 ──▶ (1) ──
                              ▲    │
                              │    │
                              │    ▼
                              └── (cycle)

Output: True
```

Complexity analysis:

- Time complexity: O(K * LogN)
- Space complexity: O(K)

Where:

- `N` is the input number.
- `K` is the number of iterations required to determine whether the number is happy or not (cycle is detected).

```python
# O(LogN) time and O(1) space
# d (number of digits) = (Log(10) n) + 1 ~ O(LogN)
def sumOfSquares(n: int) -> int:
    total = 0

    while n > 0:
        total += (n % 10) ** 2
        n = n // 10

    return total

def isHappy(n: int) -> bool:
    squares = set()
    last_sum = sumOfSquares(n)
    
    while last_sum != 1:
        if last_sum in squares:
            return False
        
        squares.add(last_sum)
        last_sum = sumOfSquares(last_sum)
    
    return True
```

## Solution 2

The solution builds upon the [Linked List Cycle](./01-linked-list-cycle.md) problem.

After detecting the cycle, we verify whether either the slow or fast pointer is pointing to `1` (indicating a happy number) or not (indicating an unhappy number).

Complexity analysis:

- Time complexity: O(K * LogN)
- Space complexity: O(1)

Where:

- `N` is the input number.
- `K` is the number of iterations required to determine whether the number is happy or not (cycle is detected).

```python
# O(LogN) time and O(1) space
# d (number of digits of number n) = (Log(10) n) + 1 ~ O(LogN)
def sumOfSquares(n: int) -> int:
    total = 0

    while n > 0:
        total += (n % 10) ** 2
        n = n // 10

    return total

def isHappy(n: int) -> bool:
    slow = n
    fast = sumOfSquares(n)
    
    while fast != slow:
        slow = sumOfSquares(slow)
        fast = sumOfSquares(sumOfSquares(fast))
    
    return True if fast == 1 else False
```
