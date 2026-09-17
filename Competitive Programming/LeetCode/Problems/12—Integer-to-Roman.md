## 12. Integer to Roman

```java
class Solution {
    private static final int[] VALUES =
        {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    private static final String[] SYMBOLS =
        {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    public String intToRoman(int num) {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < VALUES.length && num > 0; i++) {
            while (num >= VALUES[i]) {
                sb.append(SYMBOLS[i]);
                num -= VALUES[i];
            }
        }
        return sb.toString();
    }
}
```

### The idea

The trick is to treat the six subtractive forms (`CM`, `CD`, `XC`, `XL`, `IX`, `IV`) as first-class symbols rather than special cases. Once they sit in the table alongside `M`, `D`, `C`, `L`, `X`, `V`, `I`, the whole problem collapses into plain greedy: repeatedly take the largest value that fits, append its symbol, subtract.

Greedy is provably correct here because the table is built so the rules can't be violated. After subtracting a value, the remainder is always less than the next token you'd be tempted to over-use — e.g. once `900` is handled by `CM`, the remainder is under 100, so `C` can never appear four times. Same reason `V`, `L`, `D` never repeat: `4` and `9` at each place value are consumed by their subtractive pair first.

Walking `3749`: `M M M` leaves 749 → `D` leaves 249 → `C C` leaves 49 → `XL` leaves 9 → `IX`. Result: `MMMDCCXLIX`.

### Complexity

| | |
|---|---|
| Time | O(1) — num ≤ 3999, so at most 15 symbols are emitted |
| Space | O(1) |

`StringBuilder` matters over `String +=`: the latter reallocates on every append, turning a fixed-size loop into needless copying.
