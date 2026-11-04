# 29. Divide Two Integers

**Difficulty:** Medium

Given two integers `dividend` and `divisor`, divide two integers **without** using multiplication, division, and mod operator.

The integer division should truncate toward zero, which means losing its fractional part. For example, `8.345` would be truncated to `8`, and `-2.7335` would be truncated to `-2`.

Return *the quotient* after dividing `dividend` by `divisor`.

**Note:** Assume we are dealing with an environment that could only store integers within the **32-bit** signed integer range: `[-2^31, 2^31 - 1]`. For this problem, if the quotient is **strictly greater than** `2^31 - 1`, then return `2^31 - 1`, and if the quotient is **strictly less than** `-2^31`, then return `-2^31`.

---

## Solution (Repeated Doubling on Negatives, 32-bit Only)

**Time Complexity:** O(log² n)  
**Space Complexity:** O(1)

Where `n` is the absolute value of `dividend`. Each outer pass removes the largest doubled chunk of the divisor (O(log n) doublings), and there are at most O(log n) passes.

```java
class Solution {
    private static final int HALF_INT_MIN = Integer.MIN_VALUE >> 1; // -2^30

    public int divide(int dividend, int divisor) {
        // The only overflow case: -2^31 / -1 = 2^31
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }

        // Work with negatives: -2^31 has no positive counterpart in 32 bits
        int negatives = 2;
        if (dividend > 0) {
            negatives--;
            dividend = -dividend;
        }
        if (divisor > 0) {
            negatives--;
            divisor = -divisor;
        }

        int quotient = 0;
        // Both are negative, so "dividend <= divisor" means |dividend| >= |divisor|
        while (dividend <= divisor) {
            int value = divisor;   // divisor * 2^p
            int powerOfTwo = -1;   // -(2^p)

            // Double value as long as it doesn't overflow or overshoot dividend
            while (value >= HALF_INT_MIN && value + value >= dividend) {
                value += value;
                powerOfTwo += powerOfTwo;
            }

            quotient += powerOfTwo;
            dividend -= value;
        }

        // quotient is negative; flip it if the signs were the same
        return negatives != 1 ? -quotient : quotient;
    }
}
```

---

## Explanation

1. Handle the single overflow case first: `-2^31 / -1` equals `2^31`, which doesn't fit, so return `2^31 - 1`.
2. Convert both numbers to **negative**. The range of negatives is one larger than positives, so `-2^31` never needs to be negated. Track whether exactly one input was negative to know the final sign.
3. While the remaining `dividend` is at least as large in magnitude as `divisor`:
   - Start with `value = divisor` and `powerOfTwo = -1`.
   - Keep doubling both (using addition, not multiplication) while the doubled `value` still fits in `dividend`. The `HALF_INT_MIN` check prevents `value + value` from overflowing.
   - Add `powerOfTwo` to
