# 28. Find the Index of the First Occurrence in a String

**Difficulty:** Easy

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

---

## Solution (Sliding Window, O(1) Space)

**Time Complexity:** O(n · m)  
**Space Complexity:** O(1)

Where `n` is the length of `haystack` and `m` is the length of `needle`. Each of the `n - m + 1` starting positions is compared against up to `m` characters.

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length();
        int m = needle.length();

        // Try every starting position where needle could still fit
        for (int i = 0; i + m <= n; i++) {
            int j = 0;
            // Compare characters until a mismatch or a full match
            while (j < m && haystack.charAt(i + j) == needle.charAt(j)) {
                j++;
            }
            if (j == m) return i; // every character matched
        }

        return -1;
    }
}
```

---

## Explanation

1. Let `n` and `m` be the lengths of `haystack` and `needle`.
2. Try each start index `i` from `0` up to `n - m`. Past that point, `needle` would run off the end of `haystack`.
3. At each `i`, compare `haystack.charAt(i + j)` with `needle.charAt(j)` for increasing `j`, stopping at the first mismatch.
4. If `j` reaches `m`, the whole `needle` matched starting at `i`, so return `i`. Since positions are tried left to right, this is the first occurrence.
5. If no position matches, return `-1`.

If `needle` is longer than `haystack`, the loop never runs and `-1` is returned.

---

## Alternative Approach: KMP (Knuth–Morris–Pratt)

Precompute an LPS (longest proper prefix that is also a suffix) table for `needle`, so the scan never moves backward in `haystack`. Guaranteed **O(n + m)** time, **O(m)** space.

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int m = needle.length();

        // Build the LPS table
        int[] lps = new int[m];
        for (int i = 1, len = 0; i < m; ) {
            if (needle.charAt(i) == needle.charAt(len)) {
                lps[i++] = ++len;
            } else if (len > 0) {
                len = lps[len - 1];
            } else {
                lps[i++] = 0;
            }
        }

        // Scan haystack, reusing matched prefix info on mismatch
        for (int i = 0, j = 0; i < haystack.length(); ) {
            if (haystack.charAt(i) == needle.charAt(j)) {
                i++;
                j++;
                if (j == m) return i - m;
            } else if (j > 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }

        return -1;
    }
}
```

A one-line built-in also works: `return
