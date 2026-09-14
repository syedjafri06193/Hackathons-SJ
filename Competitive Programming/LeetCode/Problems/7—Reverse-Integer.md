Reverses the digits of a 32-bit signed integer, returning 0 on overflow. Since a 64-bit
type isn't allowed, each digit is checked *before* it's appended: if `result` is already
past `Integer.MAX_VALUE / 10` (or exactly at it with a digit above 7), the next multiply
would overflow. The negative side mirrors this at `-8`, because Java's `%` keeps the sign
of the dividend, so `digit` is negative when `x` is.

```java
class Solution {
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            int digit = x % 10;
            x /= 10;
            // check before multiplying, since we can't use a 64-bit type
            if (result > Integer.MAX_VALUE / 10 ||
               (result == Integer.MAX_VALUE / 10 && digit > 7)) return 0;
            if (result < Integer.MIN_VALUE / 10 ||
               (result == Integer.MIN_VALUE / 10 && digit < -8)) return 0;
            result = result * 10 + digit;
        }
        return result;
    }
}
```
