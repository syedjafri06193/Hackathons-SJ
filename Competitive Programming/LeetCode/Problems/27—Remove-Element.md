# 27. Remove Element

**Difficulty:** Easy

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` **in-place**. The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`. To get accepted, you need to:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important, nor is the size of `nums`.
- Return `k`.

---

## Solution (Two Pointers, O(1) Space)

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

Where `n` is the length of `nums`. Each element is visited exactly once.

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        // k marks where the next kept element will be written
        int k = 0;

        for (int i = 0; i < nums.length; i++) {
            // Keep every element that is not equal to val
            if (nums[i] != val) {
                nums[k] = nums[i];
                k++;
            }
        }

        return k;
    }
}
```

---

## Explanation

1. `k` marks the position where the next element to keep will be written. It starts at `0`.
2. Scan the array with `i` from start to end.
3. If `nums[i]` is not equal to `val`, copy it to `nums[k]` and increment `k`.
4. If `nums[i]` equals `val`, skip it; it will be overwritten or left beyond index `k - 1`.
5. After the loop, the first `k` slots hold all elements not equal to `val`, in their original relative order. Return `k`.

This also handles an empty array: the loop never runs and `k = 0` is returned.

---

## Alternative Approach: Swap With the End

Since order may change, replace each `val` with the last element and shrink the array. This does fewer writes when `val` is rare. Same O(n) time and O(1) space.

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int i = 0;
        int n = nums.length;
        while (i < n) {
            if (nums[i] == val) {
                nums[i] = nums[n - 1]; // overwrite with last element
                n--;                   // shrink the valid range
            } else {
                i++;
            }
        }
        return n;
    }
}
```

`i` is not advanced after a swap, because the element moved in from the end also needs to be checked.

---

## Examples

**Example 1:**
```
Input:  nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
```

**Example 2:**
```
Input:  nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
```

---

## Constraints

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`
