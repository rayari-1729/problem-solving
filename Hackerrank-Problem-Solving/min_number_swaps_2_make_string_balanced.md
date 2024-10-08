## Question :
You are given a 0-indexed string s of even length n. The string consists of exactly n / 2 opening brackets '[' and n / 2 closing brackets ']'.

A string is called balanced if and only if:
```
It is the empty string, or
It can be written as AB, where both A and B are balanced strings, or
It can be written as [C], where C is a balanced string.
You may swap the brackets at any two indices any number of times.

```
Return the minimum number of swaps to make s balanced.

 

### Example 1:
```
Input: s = "][]["
Output: 1
Explanation: You can make the string balanced by swapping index 0 with index 3.
The resulting string is "[[]]".
```
### Example 2:
```
Input: s = "]]][[["
Output: 2
Explanation: You can do the following to make the string balanced:
- Swap index 0 with index 4. s = "[]][][".
- Swap index 1 with index 5. s = "[[][]]".
The resulting string is "[[][]]".
```

## Solution : 

```python
def minSwaps(self, s: str) -> int:
    # Cancel out all the matched pairs, then we'll be left with ']]]..[[['.
    # The answer is ceil(# of unmatched pairs // 2).
    unmatched = 0

    for c in s:
      if c == '[':
        unmatched += 1
      elif unmatched > 0:  # c == ']' and there's a match.
        unmatched -= 1

    return (unmatched + 1) // 2

```
### Approach:

1. **Cancel out matched pairs** of brackets as you traverse the string.
2. After processing, you will be left with some unmatched brackets (a number of opening `[[[...` and closing `]]]...` brackets).
3. The answer is the **minimum number of swaps** needed to match these unmatched brackets.
   - Every swap of `]` and `[` will match one pair, reducing the number of unmatched brackets.
   - The number of swaps needed is `ceil(unmatched // 2)`, which is `(unmatched + 1) // 2` in integer arithmetic.

### Example:

Let’s take a string `s = "][][[]"` and walk through the function:

1. **Initial Setup**:
   - `unmatched = 0`: This variable tracks unmatched opening brackets (`[`).

2. **Traversing the String**:

   - The string is `"][][[]"`, so let’s go character by character:

   - **Step 1**: `c = ']'`  
     Since `unmatched == 0` (no unmatched opening brackets), we don't have a match for this closing bracket, so we skip it.
   
   - **Step 2**: `c = '['`  
     We found an opening bracket (`[`), so we increment `unmatched` to 1.
   
   - **Step 3**: `c = ']'`  
     Now we found a closing bracket (`]`), and we have an unmatched opening bracket (`unmatched == 1`), so we decrement `unmatched` to 0, as we’ve matched this pair.
   
   - **Step 4**: `c = '['`  
     We found another opening bracket (`[`), so we increment `unmatched` to 1.
   
   - **Step 5**: `c = '['`  
     We found another opening bracket (`[`), so we increment `unmatched` to 2.
   
   - **Step 6**: `c = ']'`  
     We found a closing bracket (`]`), and we have an unmatched opening bracket (`unmatched == 2`), so we decrement `unmatched` to 1 (matching one pair).

3. **Unmatched Brackets**:
   - After processing the string, `unmatched == 1`, meaning we have **1 unmatched opening bracket** (`[`).

4. **Minimum Swaps**:
   - To balance the unmatched brackets, the formula is `(unmatched + 1) // 2`, which in this case is `(1 + 1) // 2 = 1`.
   - We need **1 swap** to balance the string.

