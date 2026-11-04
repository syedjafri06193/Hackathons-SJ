# 30. Substring with Concatenation of All Words

**Difficulty:** Hard

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated string** is a string that exactly contains all the strings of any permutation of `words` concatenated.

- For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated string because it is not the concatenation of any permutation of `words`.

Return an array of *the starting indices* of all the concatenated substrings in `s`. You can return the answer in **any order**.

---

## Solution (Sliding Window per Offset)

**Time Complexity:** O(n · w)  
**Space Complexity:** O(m · w)

Where `n` is the length of `s`, `m` is the number of words, and `w` is the length of each word. For each of the `w` offsets, the window steps through about `n / w` word slots, and each step builds a substring of length `w`. The hash maps hold at most `m` words.

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> result = new ArrayList<>();
        int wordLen = words[0].length();
        int wordCount = words.length;
        if (s.length() < wordLen * wordCount) return result;

        // How many times each word must appear
        Map<String, Integer> need = new HashMap<>();
        for (String w : words) {
            need.merge(w, 1, Integer::sum);
        }

        // Run a separate window for each starting offset 0..wordLen-1
        for (int offset = 0; offset < wordLen; offset++) {
            Map<String, Integer> seen = new HashMap<>();
            int left = offset;
            int used = 0; // words currently in the window

            for (int right = offset; right + wordLen <= s.length(); right += wordLen) {
                String word = s.substring(right, right + wordLen);

                // Not a valid word: reset the window past it
                if (!need.containsKey(word)) {
                    seen.clear();
                    used = 0;
                    left = right + wordLen;
                    continue;
                }

                seen.merge(word, 1, Integer::sum);
                used++;

                // Too many copies of this word: shrink from the left
                while (seen.get(word) > need.get(word)) {
                    String leftWord = s.substring(left, left + wordLen);
                    seen.merge(leftWord, -1, Integer::sum);
                    used--;
                    left += wordLen;
                }

                // Window holds exactly all words
                if (used == wordCount) {
                    result.add(left);
                    // Slide forward by one word to look for the next match
                    String leftWord = s.substring(left, left + wordLen);
                    seen.merge(leftWord, -1, Integer::sum);
                    used--;
                    left += wordLen;
                }
            }
        }

        return result;
    }
}
```

---

## Explanation

1. All words have length `w`, so any match is made of `m` back-to-back chunks of length `w`. Build a map `need` counting how many times each word is required.
2. A match can start at any index, but chunks only line up within the same offset `index % w`. So run one sliding window for each offset `0` to `w - 1`.
3. For each offset, move `right` forward one word at a time and read the chunk `s[right, right + w)`:
   - If the chunk isn't in `need`, no match can include it. Clear the window and start again just after it.
   - Otherwise, add it to `seen`. If it now appears more times than needed, drop words from the left until the count is valid again.
4. When the window holds exactly `m` words, every count matches, so `left` is a valid start. Record it, then drop the leftmost word to keep searching.
5. Each chunk enters and leaves the window at most once per offset, which keeps the scan linear in the number of chunks.

---

## Alternative Approach: Check Every Start Index

For each index `i` where a full concatenation could fit, read `m` chunks and compare their counts against `need`. Simpler, but slower at **O(n · m · w)** time, **O(m · w)** space.

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> result = new ArrayList<>();
        int wordLen = words[0].length();
        int wordCount = words.length;
        int totalLen = wordLen * wordCount;

        Map<String, Integer> need = new HashMap<>();
        for (String w : words) {
            need.merge(w, 1, Integer::sum);
        }

        for (int i = 0; i + totalLen <= s.length(); i++) {
            Map<String, Integer> seen = new HashMap<>();
            int j = 0;
            while (j < wordCount) {
                String word = s.substring(i + j * wordLen, i + (j + 1) * wordLen);
                if (!need.containsKey(word)) break;
                seen.merge(word, 1, Integer::sum);
                if (seen.get(word) > need.get(word)) break;
                j++;
            }
            if (j == wordCount) result.add(i);
        }

        return result;
    }
}
```

---

## Examples

**Example 1:**
```
Input:  s = "barfoothefoobarman", words = ["foo","bar"]
Output: [0,9]
```

**Example 2:**
```
Input:  s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
Output: []
```

**Example 3:**
```
Input:  s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
Output: [6,9,12]
```

---

## Constraints

- `1 <= s.length <= 10^4`
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 30`
- `s` and `words[i]` consist of lowercase English letters.
