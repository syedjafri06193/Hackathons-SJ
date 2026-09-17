## 10. Regular Expression Matching

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        // dp[i][j] = does s[i..] match p[j..]
        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[m][n] = true;

        for (int i = m; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                boolean firstMatch = i < m
                        && (p.charAt(j) == s.charAt(i) || p.charAt(j) == '.');

                if (j + 1 < n && p.charAt(j + 1) == '*') {
                    // skip "x*" entirely, or consume one char of s and keep "x*"
                    dp[i][j] = dp[i][j + 2] || (firstMatch && dp[i + 1][j]);
                } else {
                    dp[i][j] = firstMatch && dp[i + 1][j + 1];
                }
            }
        }
        return dp[0][0];
    }
}
```

### The idea

Work backwards from the ends of both strings. At each position, look at the pattern char `p[j]` and — critically — the char *after* it, since `*` binds to the element before it.

- **No `*` following:** `p[j]` must consume exactly one char of `s`, so the answer is "does `p[j]` match `s[i]`" AND `dp[i+1][j+1]`.
- **`*` following:** two choices. Either use `x*` zero times and jump the pattern forward two (`dp[i][j+2]`), or, if `p[j]` matches `s[i]`, eat that one char of `s` and keep the same `x*` available for more (`dp[i+1][j]`).

The `i < m` guard in `firstMatch` is what lets `i == m` (string exhausted) still be a live state — that's how patterns like `a*b*c*` correctly match an empty remainder.

Iteration order matters: `i` descends because `dp[i][j]` reads `dp[i+1][*]`, and `j` descends because it reads `dp[i][j+2]`.

### Complexity

| | |
|---|---|
| Time | O(m·n) |
| Space | O(m·n), reducible to O(n) |

With m, n ≤ 20 that's trivial, but the naive recursion without memoization is exponential on inputs like `s = "aaaaaaaaaaaaaaaaaaaa"`, `p = "a*a*a*a*a*a*a*a*a*b"` — a test case LeetCode does include.

Since you only ever read row `i+1`, two rolling rows get space down to O(n).
