
# 31. Next Permutation

**Difficulty:** Medium

A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

Given an array of integers `nums`, *find the next permutation of* `nums`.

The replacement must be **in place** and use only constant extra memory.

---

## Solution (Find Pivot, Swap, Reverse, O(1) Space)

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

Where `n` is the length of `nums`. The array is scanned from the right at most twice, and the suffix is reversed once.

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;

        // 1. Find the pivot: the rightmost i with nums[i] < nums[i + 1]
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }

        if (i >= 0) {
            // 2. Find the rightmost element greater than the pivot
            int j = n - 1;
            while (nums[j] <= nums[i]) {
                j--;
            }
            // 3. Swap them
            swap(nums, i, j);
        }

        // 4. Reverse the suffix after the pivot (it is in descending order)
        reverse(nums, i + 1, n - 1);
    }

    private void swap(int[] nums, int a, int b) {
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) {
            swap(nums, left++, right--);
        }
    }
}
```

---

## Explanation

1. Scan from the right to find the **pivot** `i`, the first index where `nums[i] < nums[i + 1]`. Everything after the pivot is in descending order, so that suffix is already its largest possible arrangement.
2. If no pivot exists (`i == -1`), the whole array is descending, which is the last permutation. Skip to step 4, and reversing the whole array gives the smallest (ascending) order.
3. Otherwise, find the rightmost `j` with `nums[j] > nums[i]`. This is the smallest value in the suffix that is still larger than the pivot. Swap `nums[i]` and `nums[j]`. The suffix stays in descending order after the swap.
4. Reverse the suffix after `i` to put it in ascending order. This makes the suffix as small as possible, giving the very next permutation.

**Walkthrough with `[1,2,3]`:** pivot is `2` (index 1), the rightmost larger value is `3`, swap → `[1,3,2]`, reverse the one-element suffix → `[1,3,2]`.

---

## Alternative Approach: Find Pivot, Swap, Sort Suffix

Same idea, but sort the suffix instead of reversing it. Easier to reason about, but slower at **O(n log n)** time, and it misses the insight that the suffix is already sorted in reverse.

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }

        if (i >= 0) {
            int j = n - 1;
            while (nums[j] <= nums[i]) {
                j--;
            }
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }

        // Sort the suffix into ascending order
        Arrays.sort(nums, i + 1, n);
    }
}
```

---

## Examples

**Example 1:**
```
Input:  nums = [1,2,3]
Output: [1,3,2]
```

**Example 2:**
```
Input:  nums = [3,2,1]
Output: [1,2,3]
```

**Example 3:**
```
Input:  nums = [1,1,5]
Output: [1,5,1]
```

---

## Constraints

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`
