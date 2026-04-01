## 📌 Problem Statement  
Given an integer array `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

 You may assume:
- Exactly **one solution exists**
- You **cannot use the same element twice**
- Return answer in **any order**

Leetcode Link: https://leetcode.com/problems/two-sum/

## Examples  

### Example 1  
- **Input:** `nums = [2,7,11,15], target = 9`  
- **Output:** `[0,1]`  
- **Explanation:** `nums[0] + nums[1] = 2 + 7 = 9`

---

### Example 2  
- **Input:** `nums = [3,2,4], target = 6`  
- **Output:** `[1,2]`  

---

### Example 3  
- **Input:** `nums = [3,3], target = 6`  
- **Output:** `[0,1]`  

---

## Constraints  
- `2 <= nums.length <= 10^4`  
- `-10^9 <= nums[i] <= 10^9`  
- `-10^9 <= target <= 10^9`  
- Only one valid answer exists  

---

# Approaches  

---

## 1. Brute Force  

### Idea  
- Try every pair `(i, j)`  
- Check if `nums[i] + nums[j] == target`  

### Time Complexity  
- `O(n^2)`  

### Space Complexity  
- `O(1)`  

### Code (Java)
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[] {i, j};
                }
            }
        }
        return new int[] {};
    }
}
````

---

## 2. HashMap (Optimal)

### Idea

* Store number → index in map
* For each element:

  * Compute `complement = target - nums[i]`
  * If complement exists → return indices

---

### Visualization

<img width="992" height="530" alt="image" src="https://github.com/user-attachments/assets/f59b0770-1301-4d6d-83e2-15d88fa123e8" />

<img width="950" height="540" alt="image" src="https://github.com/user-attachments/assets/5ca785bf-7df2-4ef0-bf39-31ea8cc2ca48" />


👉 Example walkthrough:

```
nums = [2,7,11,15], target = 9

Step 1:
i=0 → num=2 → store {2:0}

Step 2:
i=1 → num=7 → complement = 2  found
→ answer = [0,1]
```

### Time Complexity

* `O(n)`

### Space Complexity

* `O(n)`

### Code (Java)

```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[] {map.get(complement), i};
            }

            map.put(nums[i], i);
        }

        return new int[] {};
    }
}
```



# Conclusion

| Approach    | Time Complexity | Space Complexity | Notes         |
| ----------- | --------------- | ---------------- | ------------- |
| Brute Force | O(n²)           | O(1)             | Not efficient |
| HashMap     | O(n)            | O(n)             | Best approach |
