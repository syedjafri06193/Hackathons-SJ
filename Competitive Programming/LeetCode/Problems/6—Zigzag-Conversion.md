```java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1) return s;

        StringBuilder[] rows = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) rows[i] = new StringBuilder();

        int cur = 0, dir = -1;
        for (char c : s.toCharArray()) {
            rows[cur].append(c);
            if (cur == 0 || cur == numRows - 1) dir = -dir;
            cur += dir;
        }

        StringBuilder res = new StringBuilder();
        for (StringBuilder row : rows) res.append(row);
        return res.toString();
    }
}
```
