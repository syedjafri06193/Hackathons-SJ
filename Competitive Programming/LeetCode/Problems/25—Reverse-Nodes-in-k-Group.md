# 25. Reverse Nodes in k-Group

**Difficulty:** Hard

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return *the modified list*.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

---

## Solution (Iterative, O(1) Space)

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

Where `n` is the number of nodes in the list. Each node is visited a constant number of times (once to check the group size, once to reverse).

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
    public ListNode reverseKGroup(ListNode head, int k) {
        // Dummy node so reversing the first group needs no special case
        ListNode dummy = new ListNode(0, head);
        ListNode groupPrev = dummy;

        while (true) {
            // Find the k-th node of the current group
            ListNode kth = groupPrev;
            for (int i = 0; i < k && kth != null; i++) {
                kth = kth.next;
            }
            if (kth == null) break; // fewer than k nodes left, leave them as is

            ListNode groupNext = kth.next;

            // Reverse the group; start prev at groupNext so the
            // reversed group's tail links to the rest of the list
            ListNode prev = groupNext;
            ListNode curr = groupPrev.next;
            while (curr != groupNext) {
                ListNode next = curr.next;
                curr.next = prev;
                prev = curr;
                curr = next;
            }

            // Connect the previous part to the new group head (kth),
            // and move groupPrev to the new group tail
            ListNode oldFirst = groupPrev.next;
            groupPrev.next = kth;
            groupPrev = oldFirst;
        }

        return dummy.next;
    }
}
```

---

## Explanation

1. Create a dummy node pointing to `head`; `groupPrev` marks the node just before the current group.
2. Walk `k` steps from `groupPrev` to find `kth`, the last node of the group. If we run out of nodes first, stop: the leftover nodes stay in their original order.
3. Save `groupNext = kth.next`, the first node after the group.
4. Reverse the `k` nodes in place. Starting `prev` at `groupNext` makes the old first node (new tail) point directly to the rest of the list.
5. Link `groupPrev.next` to `kth` (the new head of the group), then move `groupPrev` to the old first node, which is now the group's tail.
6. Repeat until fewer than `k` nodes remain.

This answers the follow-up: only a few pointers are used, so extra space is **O(1)**.

---

## Alternative Approach: Recursion

Check that `k` nodes exist, reverse them, and recursively process the rest. Simpler to read, but uses **O(n / k)** stack space.

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        // Check that there are at least k nodes
        ListNode node = head;
        for (int i = 0; i < k; i++) {
            if (node == null) return head;
            node = node.next;
        }

        // Reverse the first k nodes; node is now the (k+1)-th node
        ListNode prev = reverseKGroup(node, k);
        ListNode curr = head;
        for (int i = 0; i < k; i++) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }

        return prev;
    }
}
```

---

## Examples

**Example 1:**
```
Input:  head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**
```
Input:  head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

---

## Constraints

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

**Follow-up:** Can you solve the problem in `O(1)` extra memory space? (Yes — see the iterative solution above.)
