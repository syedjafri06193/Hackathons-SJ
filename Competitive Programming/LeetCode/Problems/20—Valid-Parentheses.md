# LeetCode 20: Valid Parentheses

## Problem Statement
Given a string $s$ containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

---

## Approach
We can solve this efficiently using a **Stack** data structure:
1. Iterate through each character of the string.
2. If the character is an opening bracket (`'('`, `'{'`, or `'['`), push it onto the stack.
3. If it's a closing bracket, check if the stack is empty. If it is, return `false`. Otherwise, pop the top element and verify that it matches the corresponding opening bracket type.
4. At the end of the loop, if the stack is empty, all opening brackets were correctly matched and closed, so return `true`; otherwise, return `false`.

---

## Java Solution

```java
class Solution {
    public boolean isValid(String s) {
        java.util.Stack<Character> stack = new java.util.Stack<>();
        for (char c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else {
                if (stack.isEmpty()) return false;
                char top = stack.pop();
                if (c == ')' && top != '(') return false;
                if (c == '}' && top != '{') return false;
                if (c == ']' && top != '[') return false;
            }
        }
        return stack.isEmpty();
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(n)$, where $n$ is the length of the string. We traverse the string of length $n$ once, and stack operations ($\text{push}$ and $\text{pop}$) take $\mathcal{O}(1)$ time.
- **Space Complexity:** $\mathcal{O}(n)$, in the worst-case scenario where the string consists entirely of opening brackets (e.g., `(((((`), the stack will store all $n$ characters.
