## 📌 Problem Statement  
Given two strings `s` and `t`, return `true` if `t` is an **anagram** of `s`, and `false` otherwise.

👉 An Anagram is a word or phrase formed by rearranging the letters of another word, using all original letters exactly once.

Leetcode Link: https://leetcode.com/problems/valid-anagram/

### Example 1  
- **Input:** `s = "anagram", t = "nagaram"`  
- **Output:** `true`  

### Example 2  
- **Input:** `s = "rat", t = "car"`  
- **Output:** `false`  

### Constraints  
- `1 <= s.length, t.length <= 5 * 10^4`  
- `s` and `t` consist of lowercase English letters  

###  Follow-up  
What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

👉 Use a `HashMap<Character, Integer>` instead of a fixed-size array, since Unicode has a much larger character set.


#  Approaches  


## 1. Sorting Approach  

### Idea  
- Convert both strings to char arrays  
- Sort them  
- Compare both arrays  

### Time Complexity  
- `O(n log n)`  

### Space Complexity  
- `O(1)` (ignoring sorting space)  

### Code (Java)  
```java
import java.util.Arrays;

class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;

        char[] sArr = s.toCharArray();
        char[] tArr = t.toCharArray();

        Arrays.sort(sArr);
        Arrays.sort(tArr);

        return Arrays.equals(sArr, tArr);
    }
}
````

## 2. HashMap Approach

### Idea

* Count frequency of characters in `s`
* Decrease using `t`
* If mismatch → return false

### Time Complexity

* `O(n)`

### Space Complexity

* `O(n)`

### Code (Java)

```java
import java.util.HashMap;

class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;

        HashMap<Character, Integer> map = new HashMap<>();

        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }

        for (char c : t.toCharArray()) {
            if (!map.containsKey(c)) return false;

            map.put(c, map.get(c) - 1);
            if (map.get(c) == 0) {
                map.remove(c);
            }
        }

        return map.isEmpty();
    }
}
```


## 3. Frequency Array (Optimal)

### Idea

* Use an array of size 26
* Increment for `s`, decrement for `t`
* If all values are 0 → anagram

### Time Complexity

* `O(n)`

### Space Complexity

* `O(1)`

### Code (Java)

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;

        int[] count = new int[26];

        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }

        for (int num : count) {
            if (num != 0) return false;
        }

        return true;
    }
}
```


# Conclusion

| Approach        | Time Complexity | Space Complexity | Notes                |
| --------------- | --------------- | ---------------- | -------------------- |
| Sorting         | O(n log n)      | O(1)             | Simple & intuitive   |
| HashMap         | O(n)            | O(n)             | Works for Unicode    |
| Frequency Array | O(n)            | O(1)             | Best (for lowercase) |
