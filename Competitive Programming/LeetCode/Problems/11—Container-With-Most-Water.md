## 11. Container With Most Water

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1;
        int best = 0;

        while (left < right) {
            int h = Math.min(height[left], height[right]);
            best = Math.max(best, h * (right - left));

            // the shorter line can never do better with a narrower width
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return best;
    }
}
```

### The idea

Area is `min(height[l], height[r]) * (r - l)` — capped by the **shorter** line, since water spills over it.

Start with the widest possible container (both ends) and shrink inward. Every move loses width, so the only way to gain is to raise the cap. Move the pointer at the shorter line: keeping it while narrowing can only produce an equal or smaller area, because the width shrinks and the height is still capped by that same short line. So no better answer is skipped by discarding it.

When the two heights are equal, either pointer works — the other line is now paired with something no taller across a smaller width, so its best pairings are already gone too.

### Complexity

| | |
|---|---|
| Time | O(n) — each step moves a pointer, total n steps |
| Space | O(1) |

The brute force over all pairs is O(n²), which times out at the upper constraint of n = 10⁵.
