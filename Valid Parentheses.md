# Two Sum

- Problem: https://leetcode.com/problems/valid-parentheses/
- Level: Easy

### Explanation

We need a way to keep track of open and close brackets, stack seems to be a perfect fit.

If we encounter an open bracket, we push that into stack, waiting for a matched close bracket to take it out.

### Code

```python
class Solution:
    def isValid(self, s: str) -> bool:
        # Use close brackets as keys
        data = { ')': '(', '}': '{', ']': '[' }
        stack = []
        
        for c in s:
            if c in data.keys():
                # Invalid string if stack is empty or no matching bracket with current close bracket
                if len(stack) == 0 or stack[-1] != data[c]:
                    return False
                stack.pop()
            else:
                # Push open bracket into stack
                stack.append(c)
                
        # Make sure that no open brackets left
        return len(stack) == 0
```

### Complexity

- Time: stack operations are O(1), and the whole solution is O(n)
- Space: O(n) due to stack and hash data
