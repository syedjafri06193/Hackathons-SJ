

```java
class Solution {
    public boolean isPalindrome(int x) {
        // Negative numbers are not palindromes
        if (x < 0) return false;
        
        // Single digit numbers are palindromes
        if (x < 10) return true;
        
        // Numbers ending with 0 (except 0 itself) are not palindromes
        if (x % 10 == 0) return false;
        
        int reversed = 0;
        int original = x;
        
        // Reverse only half of the number to avoid overflow
        while (x > reversed) {
            reversed = reversed * 10 + x % 10;
            x /= 10;
        }
        
        // For even number of digits: x == reversed
        // For odd number of digits: x == reversed / 10
        return x == reversed || x == reversed / 10;
    }
}
```
