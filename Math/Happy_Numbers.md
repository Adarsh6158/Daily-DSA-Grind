## 📌 Problem Statement
Write an algorithm to determine if a number `n` is a **happy number**.

A happy number is defined as:
- Starting with any positive integer
- Replace the number by the **sum of the squares of its digits**
- Repeat the process until:
  - The number becomes `1` → **Happy Number**
  - Or it loops endlessly in a cycle → **Not Happy**

🔗 Leetcode Link: https://leetcode.com/problems/happy-number/


## Examples

### Example 1
- **Input:** `n = 19`
- **Output:** `true`
- **Explanation:**
```

1² + 9² = 82
8² + 2² = 68
6² + 8² = 100
1² + 0² + 0² = 1

```
👉 Reached `1` → Happy Number


### Example 2
- **Input:** `n = 2`
- **Output:** `false`
- **Explanation:**
```

2 → 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4 (cycle)

````


## ⚙️ Constraints
- `1 <= n <= 2^31 - 1`


# Approaches

## 1️⃣ Using HashSet (Cycle Detection)

### Idea
- Keep track of numbers seen so far
- If number repeats → cycle detected → not happy
- If reaches `1` → happy

### Time Complexity
- `O(log n)` per iteration (digits processing)

### Space Complexity
- `O(log n)`

### Code (Java)
```java
import java.util.HashSet;

class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> seen = new HashSet<>();

        while (n != 1 && !seen.contains(n)) {
            seen.add(n);
            n = getNext(n);
        }

        return n == 1;
    }

    private int getNext(int n) {
        int sum = 0;

        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }

        return sum;
    }
}
````


## 2️⃣ Floyd’s Cycle Detection (Optimal)

### Idea

* Use **slow and fast pointers**
* If they meet → cycle exists → not happy
* If fast reaches `1` → happy

### Time Complexity

* `O(log n)`

### Space Complexity

* `O(1)`

### Code (Java)

```java
class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = n;

        do {
            slow = getNext(slow);
            fast = getNext(getNext(fast));
        } while (slow != fast);

        return slow == 1;
    }

    private int getNext(int n) {
        int sum = 0;

        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }

        return sum;
    }
}
```

## Process Flow

```
n → sum of squares → repeat
```

Example:

```
19 → 82 → 68 → 100 → 1 
```


## 🏁 Conclusion

| Approach              | Time Complexity | Space Complexity | Notes                   |
| --------------------- | --------------- | ---------------- | ----------------------- |
| HashSet               | O(log n)        | O(log n)         | Easy to understand      |
| Floyd Cycle Detection | O(log n)        | O(1)             | Optimal                 |

