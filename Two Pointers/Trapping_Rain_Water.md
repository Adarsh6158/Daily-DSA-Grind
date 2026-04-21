## 📌 Problem Statement

Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

Leetcode Link : https://leetcode.com/problems/trapping-rain-water/

## Examples

### Example 1
- **Input:** `height = [0,1,0,2,1,0,1,3,2,1,2,1]`
- **Output:** `6`

### Example 2
- **Input:** `height = [4,2,0,3,2,5]`
- **Output:** `9`

## Constraints

- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`

# Approaches

## 1. Brute Force

### Idea
- For each index, find max height on left and right
- Water = min(leftMax, rightMax) - height[i]

### Time Complexity
- `O(n^2)`

### Space Complexity
- `O(1)`

### Code (Java)
```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int water = 0;

        for (int i = 0; i < n; i++) {
            int leftMax = 0, rightMax = 0;

            for (int j = 0; j <= i; j++) {
                leftMax = Math.max(leftMax, height[j]);
            }

            for (int j = i; j < n; j++) {
                rightMax = Math.max(rightMax, height[j]);
            }

            water += Math.min(leftMax, rightMax) - height[i];
        }

        return water;
    }
}
```

## 2. Prefix and Suffix Arrays

### Idea

* Precompute leftMax and rightMax arrays
* Use them to calculate water

### Time Complexity

* `O(n)`

### Space Complexity

* `O(n)`

### Code (Java)

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;

        int[] left = new int[n];
        int[] right = new int[n];

        left[0] = height[0];
        for (int i = 1; i < n; i++) {
            left[i] = Math.max(left[i - 1], height[i]);
        }

        right[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            right[i] = Math.max(right[i + 1], height[i]);
        }

        int water = 0;
        for (int i = 0; i < n; i++) {
            water += Math.min(left[i], right[i]) - height[i];
        }

        return water;
    }
}
```

## 3. Two Pointer (Optimal)

### Idea

* Use two pointers from both ends
* Track leftMax and rightMax
* Move the smaller side

### Time Complexity

* `O(n)`

### Space Complexity

* `O(1)`

### Code (Java)

```java
class Solution {
    public int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int leftMax = 0, rightMax = 0;
        int water = 0;

        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];
                }
                left++;
            } else {
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }
                right--;
            }
        }

        return water;
    }
}
```

## 4. Stack Based

### Idea

* Use stack to store indices
* Calculate water when higher bar is found

### Time Complexity

* `O(n)`

### Space Complexity

* `O(n)`

### Code (Java)

```java
class Solution {
    public int trap(int[] height) {
        Stack<Integer> stack = new Stack<>();
        int water = 0;

        for (int i = 0; i < height.length; i++) {
            while (!stack.isEmpty() && height[i] > height[stack.peek()]) {
                int top = stack.pop();

                if (stack.isEmpty()) break;

                int distance = i - stack.peek() - 1;
                int boundedHeight = Math.min(height[i], height[stack.peek()]) - height[top];

                water += distance * boundedHeight;
            }

            stack.push(i);
        }

        return water;
    }
}
```

## Visualization

```
height = [0,1,0,2,1,0,1,3]

Water trapped between bars based on min(leftMax, rightMax)
```

## Edge Cases

* All heights are zero
* Increasing heights
* Decreasing heights
* Single bar
* No trapping possible

## Why NOT Other Approaches

### Brute Force

```java
for each index compute leftMax and rightMax
```

* `O(n^2)` time
* Not efficient

### Prefix + Suffix

```java
use extra arrays
```

* Uses extra space
* Can be optimized

## Follow-up Variations

### 1. Trapping Rain Water II (2D)

```java
use priority queue (min heap)
```

* Extend to matrix

### 2. Return water at each index

```java
int[] waterArray
```

* Store water per position

## Conclusion

| Approach        | Time Complexity | Space Complexity | Notes       |
| --------------- | --------------- | ---------------- | ----------- |
| Brute Force     | O(n^2)          | O(1)             | Not usable  |
| Prefix + Suffix | O(n)            | O(n)             | Good        |
| Two Pointer     | O(n)            | O(1)             | Optimal     |
| Stack           | O(n)            | O(n)             | Alternative |