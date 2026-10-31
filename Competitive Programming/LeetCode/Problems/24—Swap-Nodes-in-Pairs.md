# 24. Swap Nodes in Pairs

**Difficulty:** Medium

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed).

---

## Solution (Iterative with Dummy Node)

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

Where `n` is the number of nodes in the list.

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
    public ListNode swapPairs(ListNode head) {
        // Dummy node so swapping the head needs no special case
        ListNode dummy = new ListNode(0, head);
        ListNode prev = dummy;

        while (prev.next != null && prev.next.next != null) {
            ListNode first = prev.next;
            ListNode second = first.next;

            // Swap the pair
            first.next = second.next;
            second.next = first;
            prev.next = second;

            // Move to the node before the next pair
            prev = first;
        }

        return dummy.next;
    }
}
```

---

## Explanation

1. Create a dummy node pointing to `head`, and set `prev` to the dummy.
2. While there are at least two nodes after `prev`, call them `first` and `second`.
3. Rewire the pointers so `prev → second → first → (rest of list)`.
4. Move `prev` to `first`, which is now the second node of the swapped pair.
5. Stop when fewer than two nodes remain; an odd leftover node stays in place.

The dummy node removes the special case of swapping the original head, and `dummy.next` is the new head.

---

## Alternative Approach: Recursion

Swap the first two nodes, then recursively swap the rest of the list. This is shorter but uses **O(n)** stack space.

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode second = head.next;
        head.next = swapPairs(second.next);
        second.next = head;

        return second;
    }
}
```

---

## Examples

**Example 1:**
```
Input:  head = [1,2,3,4]
Output: [2,1,4,3]
```

**Example 2:**
```
Input:  head = []
Output: []
```

**Example 3:**
```
Input:  head = [1]
Output: [1]
```

**Example 4:**
```
Input:  head = [1,2,3]
Output: [2,1,3]
```

---

## Constraints

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`
