# 23. Merge k Sorted Lists

**Difficulty:** Hard

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

---

## Solution (Min-Heap / PriorityQueue)

**Time Complexity:** O(N log k)  
**Space Complexity:** O(k)

Where `N` is the total number of nodes across all lists and `k` is the number of lists.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }

        // Min-heap ordered by node value
        PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);

        // Add the head of every non-empty list
        for (ListNode node : lists) {
            if (node != null) {
                minHeap.offer(node);
            }
        }

        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (!minHeap.isEmpty()) {
            ListNode smallest = minHeap.poll();
            tail.next = smallest;
            tail = tail.next;

            // If the chosen list still has more nodes, push the next one
            if (smallest.next != null) {
                minHeap.offer(smallest.next);
            }
        }

        return dummy.next;
    }
}
```

---

## Explanation

1. Create a min-heap that always gives us the node with the smallest value.
2. Insert the head of every non-empty list into the heap.
3. Repeatedly extract the smallest node and append it to the result.
4. Whenever we take a node from a list, we push its next node (if any) into the heap.
5. Continue until the heap is empty.

This guarantees that we always pick the globally smallest remaining node, producing a fully sorted merged list.

---

## Alternative Approach: Divide and Conquer

You can also merge the lists pairwise (similar to merge-sort). This also runs in **O(N log k)** time and often has better constant factors / cache behavior.

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        return merge(lists, 0, lists.length - 1);
    }

    private ListNode merge(ListNode[] lists, int left, int right) {
        if (left == right) return lists[left];
        if (left + 1 == right) return mergeTwo(lists[left], lists[right]);

        int mid = left + (right - left) / 2;
        ListNode l1 = merge(lists, left, mid);
        ListNode l2 = merge(lists, mid + 1, right);
        return mergeTwo(l1, l2);
    }

    private ListNode mergeTwo(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                tail.next = l1;
                l1 = l1.next;
            } else {
                tail.next = l2;
                l2 = l2.next;
            }
            tail = tail.next;
        }

        tail.next = (l1 != null) ? l1 : l2;
        return dummy.next;
    }
}
```

---

## Examples

**Example 1:**
```
Input:  lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
```

**Example 2:**
```
Input:  lists = []
Output: []
```

**Example 3:**
```
Input:  lists = [[]]
Output: []
```
