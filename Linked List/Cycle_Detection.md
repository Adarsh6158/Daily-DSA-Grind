## 📌 Problem Statement

Given the head of a linked list, determine if the linked list has a cycle in it.

A cycle exists if some node in the list can be reached again by continuously following the `next` pointer.

LeetCode Link: [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)


## Examples

### Example 1

* **Input:** `head = [3,2,0,-4], pos = 1`
* **Output:** `true`
* **Explanation:** Tail connects to node at index 1


### Example 2

* **Input:** `head = [1,2], pos = 0`
* **Output:** `true`
* **Explanation:** Tail connects to head


### Example 3

* **Input:** `head = [1], pos = -1`
* **Output:** `false`
* **Explanation:** No cycle


## Constraints

* Number of nodes: `0 <= n <= 10^4`
* Node values: `-10^5 <= Node.val <= 10^5`
* `pos` is used internally (not passed as parameter)


# Approaches

## 1. Brute Force (Using HashSet)

### Idea

* Traverse the list
* Store each node in a `HashSet`
* If a node is already visited → cycle exists


### Time Complexity

* `O(n)`

### Space Complexity

* `O(n)`


### Code (Java)

```java
import java.util.HashSet;

public class Solution {
    public boolean hasCycle(ListNode head) {
        HashSet<ListNode> set = new HashSet<>();

        while (head != null) {
            if (set.contains(head)) {
                return true;
            }
            set.add(head);
            head = head.next;
        }

        return false;
    }
}
```


## 2. Floyd’s Cycle Detection (Tortoise & Hare)

### Idea

* Use two pointers:

  * `slow` moves 1 step
  * `fast` moves 2 steps
* If cycle exists → they will meet
* If `fast` reaches null → no cycle


### Time Complexity

* `O(n)`

### Space Complexity

* `O(1)`


### Code (Java)

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;      
            fast = fast.next.next;      

            if (slow == fast) {
                return true;            
            }
        }

        return false;
    }
}
```

## Conclusion

| Approach          | Time Complexity | Space Complexity | Notes                           |
| ----------------- | --------------- | ---------------- | ------------------------------- |
| HashSet           | O(n)            | O(n)             | Easy but uses extra space       |
| Floyd’s Algorithm | O(n)            | O(1)             | Optimal                         |


