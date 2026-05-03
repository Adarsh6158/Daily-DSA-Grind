## 📌 Problem Statement

Design an algorithm to encode a list of strings into a single string and decode it back.

You must ensure:
- The encoded string can be decoded back to the original list
- Strings may contain any characters

Leetcode Link : https://leetcode.com/problems/encode-and-decode-strings/


## Examples

### Example 1
- **Input:** `["neet","code","love","you"]`
- **Output:** `["neet","code","love","you"]`

### Example 2
- **Input:** `["we","say",":","yes"]`
- **Output:** `["we","say",":","yes"]`


## Constraints

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- Strings may contain any ASCII characters


# Approaches

## 1. Length Prefix Encoding (Optimal)

### Idea
- Store each string as `<length>#<string>`
- Decode by reading length and extracting string

### Time Complexity
- `O(n)`

### Space Complexity
- `O(n)`

### Code (Java)
```java
public class Codec {

    public String encode(List<String> strs) {
        StringBuilder encoded = new StringBuilder();

        for (String str : strs) {
            encoded.append(str.length()).append("#").append(str);
        }

        return encoded.toString();
    }

    public List<String> decode(String s) {
        List<String> result = new ArrayList<>();
        int i = 0;

        while (i < s.length()) {
            int j = i;

            while (s.charAt(j) != '#') {
                j++;
            }

            int length = Integer.parseInt(s.substring(i, j));
            j++;

            String word = s.substring(j, j + length);
            result.add(word);

            i = j + length;
        }

        return result;
    }
}
```


## 2. Delimiter Based Encoding

### Idea
- Join strings using a delimiter

### Time Complexity
- `O(n)`

### Space Complexity
- `O(n)`

### Code (Java)
```java
public class Codec {

    public String encode(List<String> strs) {
        return String.join("#", strs);
    }

    public List<String> decode(String s) {
        return Arrays.asList(s.split("#"));
    }
}
```


## 3. Fixed Length Encoding

### Idea
- Use fixed size to store length

### Time Complexity
- `O(n)`

### Space Complexity
- `O(n)`

### Code (Java)
```java
public class Codec {

    private static final int SIZE = 5;

    public String encode(List<String> strs) {
        StringBuilder encoded = new StringBuilder();

        for (String str : strs) {
            String len = String.format("%05d", str.length());
            encoded.append(len).append(str);
        }

        return encoded.toString();
    }

    public List<String> decode(String s) {
        List<String> result = new ArrayList<>();
        int i = 0;

        while (i < s.length()) {
            int length = Integer.parseInt(s.substring(i, i + SIZE));
            i += SIZE;

            String word = s.substring(i, i + length);
            result.add(word);

            i += length;
        }

        return result;
    }
}
```


## 4. Escape Character Encoding

### Idea
- Escape special characters and use delimiter

### Time Complexity
- `O(n)`

### Space Complexity
- `O(n)`

### Code (Java)
```java
public class Codec {

    public String encode(List<String> strs) {
        StringBuilder encoded = new StringBuilder();

        for (String str : strs) {
            str = str.replace("/", "//").replace("#", "/#");
            encoded.append(str).append("#");
        }

        return encoded.toString();
    }

    public List<String> decode(String s) {
        List<String> result = new ArrayList<>();
        StringBuilder current = new StringBuilder();

        int i = 0;
        while (i < s.length()) {
            char c = s.charAt(i);

            if (c == '/') {
                current.append(s.charAt(i + 1));
                i += 2;
            } else if (c == '#') {
                result.add(current.toString());
                current.setLength(0);
                i++;
            } else {
                current.append(c);
                i++;
            }
        }

        return result;
    }
}
```


## Visualization

### Encoding

```
["hello","world"]
↓
"5#hello5#world"
```

### Decoding

```
5#hello5#world

→ length = 5 → "hello"
→ length = 5 → "world"
```


## Edge Cases

- Empty list → `""`
- Empty string inside list
- Strings containing special characters like `#`, `/`
- Single string
- Large input size


## Why NOT Other Approaches

### Using Delimiter

```java
String.join("#", strs);
```

```
["ab#c","d"] → "ab#c#d"
```

- Cannot correctly split during decoding


### Fixed Length

```java
String.format("%05d", str.length());
```

- Wastes space
- Assumes fixed max length
- Not scalable


### Escape Characters

```java
str.replace("/", "//").replace("#", "/#");
```

- Complex encoding and decoding
- Error-prone in edge cases


## Follow-up Variations

### 1. Optimize decoding without substring

```java
public List<String> decode(String s) {
    List<String> result = new ArrayList<>();
    int i = 0;

    while (i < s.length()) {
        int j = i;

        while (s.charAt(j) != '#') {
            j++;
        }

        int length = Integer.parseInt(s.substring(i, j));
        j++;

        StringBuilder word = new StringBuilder();
        for (int k = j; k < j + length; k++) {
            word.append(s.charAt(k));
        }

        result.add(word.toString());
        i = j + length;
    }

    return result;
}
```


### 2. Handle very large inputs using streaming

```java
public List<String> decode(InputStream input) throws IOException {
    List<String> result = new ArrayList<>();
    StringBuilder number = new StringBuilder();

    int ch;
    while ((ch = input.read()) != -1) {
        char c = (char) ch;

        if (c == '#') {
            int length = Integer.parseInt(number.toString());
            number.setLength(0);

            StringBuilder word = new StringBuilder();
            for (int i = 0; i < length; i++) {
                word.append((char) input.read());
            }

            result.add(word.toString());
        } else {
            number.append(c);
        }
    }

    return result;
}
```


### 3. Message serialization concept

```java
public String encode(List<String> strs) {
    StringBuilder encoded = new StringBuilder();

    for (String str : strs) {
        encoded.append(str.length()).append("#").append(str);
    }

    return encoded.toString();
}
```

```
["hello","world"]
↓
"5#hello5#world"
```

## Conclusion

| Approach               | Time Complexity | Space Complexity | Notes               |
| ---------------------- | --------------- | ---------------- | ------------------- |
| Length Prefix Encoding | O(n)            | O(n)             | Optimal             |
| Delimiter Based        | O(n)            | O(n)             | Fails in edge cases |
| Fixed Length           | O(n)            | O(n)             | Not scalable        |
| Escape Character       | O(n)            | O(n)             | Complex             |
