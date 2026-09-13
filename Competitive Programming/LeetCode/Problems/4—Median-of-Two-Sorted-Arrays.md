
```java
class Solution {

    /**
     * @param nums1 sorted ascending
     * @param nums2 sorted ascending
     * @return the median of the two arrays combined
     * @throws IllegalArgumentException if the inputs are empty or not sorted
     */
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        // Always binary search over the shorter array.
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length;
        int n = nums2.length;
        int half = (m + n + 1) / 2;

        int lo = 0;
        int hi = m;

        while (lo <= hi) {
            int i = (lo + hi) / 2;  // elements taken from nums1
            int j = half - i;       // elements taken from nums2

            int left1 = (i == 0) ? Integer.MIN_VALUE : nums1[i - 1];
            int right1 = (i == m) ? Integer.MAX_VALUE : nums1[i];
            int left2 = (j == 0) ? Integer.MIN_VALUE : nums2[j - 1];
            int right2 = (j == n) ? Integer.MAX_VALUE : nums2[j];

            if (left1 <= right2 && left2 <= right1) {
                int maxLeft = Math.max(left1, left2);
                if (((m + n) & 1) == 1) {
                    return maxLeft;
                }
                int minRight = Math.min(right1, right2);
                return ((long) maxLeft + minRight) / 2.0;
            }

            if (left1 > right2) {
                hi = i - 1;  // took too much from nums1
            } else {
                lo = i + 1;  // took too little from nums1
            }
        }

        throw new IllegalArgumentException("Inputs must be non-empty and sorted ascending");
    }
}
```
