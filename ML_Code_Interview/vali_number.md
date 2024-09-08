# Valid Number

## Problem Description

Validate if a given string is a numeric value.

### Examples:

- `"0"` -> `true`
- `"0.1"` -> `true`
- `"abc"` -> `false`

### Rules:

1. Ignore leading and trailing whitespaces.
2. The number can have an optional `+` or `-` sign at the start.
3. A valid number can be an integer or a decimal.
4. A decimal must have digits before or after the decimal point (or both).
5. Exponential notation (`e`) is valid only if followed by an integer (e.g., "2e10").

## Frequently Asked Questions:

**Q: Do we ignore spaces within the number?**  
A: No, only ignore leading and trailing spaces. `"1 1"` is not numeric.

**Q: Is a sign allowed before the number?**  
A: Yes, both `+` and `-` signs are allowed at the start, like `"+1"` or `"-1"`.

**Q: What about other bases like hexadecimal (e.g., "0xFF")?**  
A: Only decimal numbers are valid. `"0xFF"` is not considered numeric.

**Q: Are exponents like `"1e10"` valid?**  
A: Yes, the exponent part is valid if it follows the rules (e.g., digits must come after `e`).

## Solution

To solve this problem, we break down the string into parts:

1. **Leading Whitespaces** (optional).
2. **Sign (+/-)** (optional).
3. **Number** (mandatory).
4. **Trailing Whitespaces** (optional).

A number can be either:
- An **integer** with only digits.
- A **decimal** with digits, a decimal point, and more digits.

To be valid:
- At least one digit must be present.
- Exponents ('e') must be followed by an integer.

### Python Code Solution

Here is the Python code to check if a string is numeric:

```python
def is_number(s: str) -> bool:
    # Remove leading and trailing whitespaces
    s = s.strip()
    if not s:
        return False

    # Track the state of processing
    i, n = 0, len(s)
    is_numeric = False

    # Check for an optional sign at the start
    if i < n and (s[i] == '+' or s[i] == '-'):
        i += 1

    # Process digits before the decimal point
    while i < n and s[i].isdigit():
        i += 1
        is_numeric = True

    # Check for a decimal point and digits after it
    if i < n and s[i] == '.':
        i += 1
        while i < n and s[i].isdigit():
            i += 1
            is_numeric = True

    # Check for an exponent 'e' and process the exponent part
    if is_numeric and i < n and s[i] == 'e':
        i += 1
        is_numeric = False  # reset to check the exponent part

        # Optional sign after 'e'
        if i < n and (s[i] == '+' or s[i] == '-'):
            i += 1

        # Process digits in the exponent part
        while i < n and s[i].isdigit():
            i += 1
            is_numeric = True

    # Return true if all characters are processed and valid
    return is_numeric and i == n
```
# Test cases
print(is_number("0"))      # True
print(is_number("0.1"))    # True
print(is_number("abc"))    # False


## Further Thoughts
Potential Issues and Enhancements:
Invalid Exponential Formats: While the current solution handles most cases well, it assumes that the exponent is properly formatted (e.g., "2e10"). Additional checks might be needed to ensure that no invalid formats like "2e" or "e10" are allowed.

### Edge Cases: 
Consider special cases like multiple decimal points ("1.2.3"), signs in the wrong place ("++1", "1e+e10"), or strings with invalid characters ("1a", "1e10.5").

### Performance Considerations:
For extremely large strings, consider optimizing the string operations to reduce time complexity. However, for most practical purposes, this solution is efficient.

### Regular Expressions (Regex): 
Another approach could be using regular expressions to validate the format. This can make the code shorter and more expressive but might be harder to understand for beginners.

## Improved Solution:
You can use regular expressions to succinctly handle all the cases in a more readable way:
