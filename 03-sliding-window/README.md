# Pattern: Sliding Window

## Overview

The Sliding Window technique keeps a contiguous range — a *window* — over an array or a string, and slides it forward instead of re-examining the same elements again and again.

The window is described by two boundaries, `window_start` and `window_end`, plus a small summary of what is inside it: a running sum, a count, or a frequency map. When a value enters on the right, you fold it into the summary and when a value leaves on the left, you remove it. Nothing else gets recomputed.

That turns the obvious nested-loop solution, which re-scans forward from every index, into a single pass.

## Example

Given `[1, 3, 2, 6, -1, 4, 1, 8, 2]` and `K = 5`, find the average of every group of five neighbouring values.

The word *neighbouring* is the important one: the values in each group must sit next to each other. A group like that is a contiguous subarray.

The obvious solution starts at every index and adds the next `K` values. That costs `K` additions per group, so $O(n \cdot k)$ overall — and it repeats an enormous amount of work, because consecutive groups share `K - 1` of their values:

```plaintext
index      0   1   2   3   4   5   6   7   8
value      1   3   2   6  -1   4   1   8   2

           └───────────────┘                  sum = 11                  avg = 2.2
               └───────────────┘              sum = 11 - 1 + 4 = 14     avg = 2.8
                   └───────────────┘          sum = 14 - 3 + 1 = 12     avg = 2.4
                       └───────────────┘      sum = 12 - 2 + 8 = 18     avg = 3.6
                           └───────────────┘  sum = 18 - 6 + 2 = 14     avg = 2.8
```

Each step subtracts the value that left and adds the value that entered — two operations instead of `K`. The whole scan becomes $O(n)$.

## Core idea

A window is a range plus a summary. Only two things ever happen to it:

- a value enters on the right, and the summary absorbs it
- a value leaves on the left, and the summary forgets it

A window's summary can be updated from its neighbour's in $O(1)$, so you never rebuild it from scratch. That single fact is what the whole pattern rests on.

Every sliding-window problem is those same two moves, with three decisions filled in:

1. The summary — what you keep about the window. A sum, a count of zeros, a frequency map.
2. The validity condition — what makes a window acceptable. "At most `k` distinct keys", "sum at least `target`", "no duplicates".
3. What you record — the longest length, the shortest length, a start index, a boolean.

The window length is always `window_end - window_start + 1`.

### Why it is $O(n)$

`window_start` never moves backwards. Each index enters the window exactly once and leaves at most once, so the inner shrink loop runs at most `n` times *in total* across the entire outer loop — not once per outer iteration. Two pointers each walking forward at most `n` steps is $2n$ moves, which is $O(n)$.

This is why a nested `while` inside a `for` is still linear here, even though it looks quadratic.

## Variations

### 1. Fixed-size window

The window length is pinned at `k` from the start. There is no decision about when to shrink — you shrink on every step once the window is full.

```plaintext
value      2   1   5   1   3   2        k = 3

           └───────┘                    sum = 8
               └───────┘                sum = 8 - 2 + 1 = 7
                   └───────┘            sum = 7 - 1 + 3 = 9   ← best
                       └───────┘        sum = 9 - 5 + 2 = 6
```

Use it when:

- The problem hands you a fixed size `k`
- You want the best sum, average, or count over every window of that size

### 2. Longest valid window

Grow on every step. When the window becomes invalid, shrink from the left until it is valid again, then record the length. The window never shrinks below the best it has seen, so the answer emerges naturally.

```plaintext
s = "araaci",  k = 2 distinct

a        {a:1}              valid    length 1
ar       {a:1, r:1}         valid    length 2
ara      {a:2, r:1}         valid    length 3
araa     {a:3, r:1}         valid    length 4  ← best
araac    {a:3, r:1, c:1}    3 > 2, shrink from the left
 raac    {a:2, r:1, c:1}    still 3, shrink
  aac    {a:2, c:1}         valid    length 3
```

Use it when:

- You want the longest range satisfying a constraint
- The constraint is an upper bound: "at most `k` distinct", "at most `k` replacements"

### 3. Shortest valid window

The mirror image. Grow until the window becomes valid, then shrink as far as you can while it stays valid, recording the length each time. Here the `while` loop runs on validity, not invalidity.

```plaintext
nums = [2, 1, 5, 2, 3, 2],  target = 7

[2]            sum = 2    < 7, grow
[2, 1]         sum = 3    < 7, grow
[2, 1, 5]      sum = 8   >= 7, record length 3, drop 2
   [1, 5]      sum = 6    < 7, grow
   [1, 5, 2]   sum = 8   >= 7, record length 3, drop 1
      [5, 2]   sum = 7   >= 7, record length 2, drop 5  ← best
         [2]   sum = 2    < 7, grow
```

Use it when:

- You want the shortest range satisfying a constraint
- The constraint is a lower bound: "sum at least `target`", "contains all of the pattern"

### 4. Fixed window with frequency matching

A fixed window again, but the summary is a frequency map compared against a target map. Instead of re-comparing two maps on every step — which would cost $O(\Sigma)$ per index — keep a single counter of how many characters are fully matched.

```plaintext
s = "oidbcaf",  pattern = "abc"     window size 3

oid    matched 0
 idb   matched 1  (b)
  dbc  matched 2  (b, c)
   bca matched 3  ← permutation found
```

Use it when:

- You need a permutation, an anagram, or an exact multiset match
- The window size is the pattern's length

## Templates

A fixed-size window shrinks unconditionally once the window is full:

```python
window_start = 0
window_sum = 0

for window_end in range(len(nums)):
    window_sum += nums[window_end]              # value enters on the right

    if window_end >= k - 1:                     # window is exactly k wide
        best = update(best, window_sum)
        window_sum -= nums[window_start]        # value leaves on the left
        window_start += 1
```

A variable-size window grows every step, shrink only while the window is invalid:

```python
window_start = 0
summary = empty()

for window_end in range(len(nums)):
    add(summary, nums[window_end])

    while not valid(summary):                   # `while`, not `if`
        remove(summary, nums[window_start])
        window_start += 1

    best = update(best, window_end - window_start + 1)
```

Use `while` rather than `if` when shrinking: removing a single value may not be enough to restore validity, and one `if` would leave the window in a broken state for the rest of the scan.

For the *shortest*-window variant, invert the loop — shrink while the window is valid, recording the length before each removal.

## Recognize it when

- The answer is a contiguous subarray or substring
- You want the longest, shortest, or best range satisfying some condition
- The problem hands you a fixed size `k`
- Your first instinct is a nested loop that re-scans forward from each index
- A small summary — a sum, a count, a frequency map — fully describes a range

Words like *consecutive*, *neighbouring*, *substring* and *subarray* are the strong signals: they tell you elements cannot be skipped.

## Reach for something else when

- Elements may be skipped: a subsequence rather than a subarray, and usually Dynamic Programming.
- The array is sorted and you need a pair or a triplet: [Two Pointers](../01-two-pointers/README.md) converges faster.
- There are many independent range-sum queries: precompute a Prefix Sum instead.
- You need the maximum or minimum *inside* every window: a Monotonic Deque maintains that in $O(1)$ amortised; a plain sum or counter cannot.
- The array contains negative numbers and you want "subarray sum equals `k`": growing the window no longer increases the sum monotonically, so shrinking proves nothing. Use a Prefix Sum with a hash map.

That last point is exactly why [Smallest Subarray with a Given Sum](./02-smallest-subarray-with-a-given-sum.md) insists on positive numbers, and it is the trap hiding in that problem.

## Pitfalls

- Shrinking with `if` instead of `while`. One removal may not restore validity.
- Recording the answer in the wrong place. For a longest window, record *after* shrinking. For a shortest window, record *before* removing.
- Forgetting to delete zero-count keys from a frequency map. `len(counter)` is the number of distinct characters only if exhausted keys are popped.
- Seeding the answer with `0`. For a minimum, seed with `float("inf")`; for a maximum over values that can be negative, seed with `float("-inf")`.
- Assuming the window shrinks from both ends. It never does — `window_start` only moves forward.

## Key takeaways

- A window is two boundaries plus a summary you can update in $O(1)$.
- The pattern is always the same two moves; only the summary, the validity test, and the recorded answer change.
- A `while` loop nested in a `for` is still $O(n)$ here, because `window_start` only ever moves forward.
- Longest-window problems shrink while *invalid*; shortest-window problems shrink while *valid*.
- "Contiguous" is the keyword that makes the pattern applicable; "skippable" is the keyword that rules it out.

## Problems

See [PROBLEMS.md](./PROBLEMS.md) for the full list, including a short set to revise when time is tight.
