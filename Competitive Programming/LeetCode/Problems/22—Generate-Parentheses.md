# 22. Generate Parentheses

**Difficulty:** Medium

Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

## Example 1:
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

## Example 2:
```
Input: n = 1
Output: ["()"]
```

## Constraints:
- `1 <= n <= 8`

---

## Solution (Java)

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        backtrack(result, new StringBuilder(), 0, 0, n);
        return result;
    }
    
    private void backtrack(List<String> result, StringBuilder current, int open, int close, int n) {
        if (current.length() == n * 2) {
            result.add(current.toString());
            return;
        }
        
        if (open < n) {
            current.append('(');
            backtrack(result, current, open + 1, close, n);
            current.deleteCharAt(current.length() - 1);
        }
        
        if (close < open) {
            current.append(')');
            backtrack(result, current, open, close + 1, n);
            current.deleteCharAt(current.length() - 1);
        }
    }
}
```

### Approach
We use **backtracking** to build valid combinations:

- Track the count of open `(` and close `)` parentheses used so far.
- At each step:
  - Add `(` if we still have opens remaining (`open < n`).
  - Add `)` only if it keeps the string valid (`close < open`).
- When the string reaches length `2 * n`, it's a complete valid combination.

This efficiently generates all solutions (the number of valid combinations is the Catalan number).
```

The file has been created at:

**`/home/workdir/artifacts/Generate-Parentheses.md`**

You can download it from there.
