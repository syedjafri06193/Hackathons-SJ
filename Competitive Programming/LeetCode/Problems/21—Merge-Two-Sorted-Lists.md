# LeetCode 21: Merge Two Sorted Lists

## Problem Statement

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

## Approach

We can solve this efficiently using an iterative approach with a **dummy node**:

1. Create a `dummy` node to serve as the start of our new merged list, which simplifies edge cases (like starting with an empty list).
2. Use a `current` pointer to keep track of the tail of our merged list.
3. Compare the values of the nodes at the heads of `list1` and `list2`. Attach the smaller node to `current.next` and advance that list's pointer.
4. Move the `current` pointer forward.
5. Repeat this until one of the lists becomes empty. Once that happens, attach the remaining nodes of the non-empty list to the end of our merged list.
6. Return `dummy.next`, which points to the true head of our newly merged sorted list.

## Java Solution

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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(-1);
        ListNode current = dummy;
        
        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                current.next = list1;
                list1 = list1.next;
            } else {
                current.next = list2;
                list2 = list2.next;
            }
            current = current.next;
        }
        
        if (list1 != null) {
            current.next = list1;
        } else {
            current.next = list2;
        }
        
        return dummy.next;
    }
}
```

## Complexity Analysis

* **Time Complexity:** $\mathcal{O}(n + m)$, where $n$ and $m$ are the lengths of `list1` and `list2` respectively. We traverse each list at most once.
* **Space Complexity:** $\mathcal{O}(1)$, because we are only rearranging the pointers of the existing nodes rather than creating new ones.
