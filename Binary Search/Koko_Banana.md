## 📌 Problem Statement

Koko loves to eat bananas. There are `n` piles of bananas, where the `i-th` pile has `piles[i]` bananas. The guards have gone and will return in `h` hours.

Koko can decide her eating speed `k` (bananas per hour). Each hour, she chooses a pile and eats `k` bananas from it. If the pile has less than `k` bananas, she eats all of them instead.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

LeetCode Link: [https://leetcode.com/problems/koko-eating-bananas/](https://leetcode.com/problems/koko-eating-bananas/)

## Examples

### Example 1

* **Input:** `piles = [3,6,7,11], h = 8`
* **Output:** `4`


### Example 2

* **Input:** `piles = [30,11,23,4,20], h = 5`
* **Output:** `30`


### Example 3

* **Input:** `piles = [30,11,23,4,20], h = 6`
* **Output:** `23`


## Constraints

* `1 <= piles.length <= 10^4`
* `piles.length <= h <= 10^9`
* `1 <= piles[i] <= 10^9`


# Approaches

## 1. Brute Force

### Idea

* Try every possible eating speed from `1` to `max(piles)`
* For each speed, calculate total hours required
* Return the minimum valid speed


### Time Complexity

* `O(n * max(piles))`

### Space Complexity

* `O(1)`


### Code (Java)

```java id="bfkoko"
public class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int max = 0;
        for (int pile : piles) {
            max = Math.max(max, pile);
        }

        for (int k = 1; k <= max; k++) {
            int hours = 0;

            for (int pile : piles) {
                hours += (int) Math.ceil((double) pile / k);
            }

            if (hours <= h) {
                return k;
            }
        }

        return max;
    }
}
```


## 2. Binary Search (Optimal)

### Idea

* Search space is `k = 1` to `max(piles)`
* Use binary search to find minimum valid `k`
* For each mid, check if Koko can finish within `h` hours


### Time Complexity

* `O(n log max(piles))`

### Space Complexity

* `O(1)`


### Code (Java)

```java id="optkoko"
public class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1;
        int right = 0;

        for (int pile : piles) {
            right = Math.max(right, pile);
        }

        while (left < right) {
            int mid = left + (right - left) / 2;

            int hours = 0;
            for (int pile : piles) {
                hours += (pile + mid - 1) / mid;
            }

            if (hours <= h) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }
}
```


## Conclusion

| Approach      | Time Complexity     | Space Complexity | Notes            |
| ------------- | ------------------- | ---------------- | ---------------- |
| Brute Force   | O(n * max(piles))   | O(1)             | Too slow         |
| Binary Search | O(n log max(piles)) | O(1)             | Optimal solution |
