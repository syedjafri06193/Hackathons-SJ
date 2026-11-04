# 26. Remove Duplicates from Sorted Array

**Difficulty:** Easy

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates **in-place** such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Consider the number of *unique elements* in `nums` to be `k`. After removing duplicates, return the number of unique elements `k`.

The first `k` elements of `nums` should contain the unique numbers in **sorted order**. The remaining elements beyond index `k - 1` can be ignored.

---

## Solution (Two Pointers, O(1) Space)

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

Where `n` is the length of `nums`. Each element is visited exactly once.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // The first element is always unique
        int k = 1;

        for (int i = 1; i < nums.length; i++) {
            // Sorted array: a new value differs from the last unique one written
            if (nums[i] != nums[k - 1]) {
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

1. `k` marks the position where the next unique value will be written. It starts at `1` because `nums[0]` is always unique.
2. Scan the array with `i` starting from index `1`.
3. Because the array is sorted, duplicates are always adjacent. So `nums[i]` is a new value exactly when it differs from `nums[k - 1]`, the last unique value written.
4. When a new value is found, copy it to `nums[k]` and increment `k`.
5. After the loop, the first `k` slots hold the unique values in sorted order. Return `k`.

Starting `k` at `1` is safe because the constraints guarantee at least one element.

---

## Alternative Approach: Compare With Previous Element

Instead of comparing against the last written value, compare each element with the one directly before it. Same O(n) time and O(1) space.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int k = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[i - 1]) {
                nums[k++] = nums[i];
            }
        }
        return k;
    }
}
```

This works because `nums[i - 1]` is read before it could ever be overwritten (`k <= i` at all times).

---

## Examples

**Example 1:**
```
Input:  nums = [1,1,2]
Output: 2, nums = [1,2,_]
```

**Example 2:**
```
Input:  nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
```

---

## Constraints

- `1 <= nums.length <= 3 * 10^4`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in **non-decreasing** order.
