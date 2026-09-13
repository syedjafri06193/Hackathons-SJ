



```java
import java.util.Arrays;

class Solution {

    private static final int ALPHABET_SIZE = 128; // ASCII only

    public int lengthOfLongestSubstring(String s) {
        int[] lastSeen = new int[ALPHABET_SIZE];
        Arrays.fill(lastSeen, -1);

        int best = 0;
        int start = 0;

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (lastSeen[c] >= start) {
                start = lastSeen[c] + 1;
            }
            lastSeen[c] = i;
            best = Math.max(best, i - start + 1);
        }

        return best;
    }
}
```
