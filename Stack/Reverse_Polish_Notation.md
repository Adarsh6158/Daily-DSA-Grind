## 📌 Problem Statement

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression.

Division between two integers should truncate toward zero.

Leetcode Link : https://leetcode.com/problems/evaluate-reverse-polish-notation/

## Examples

### Example 1
- **Input:** `tokens = ["2","1","+","3","*"]`
- **Output:** `9`

### Example 2
- **Input:** `tokens = ["4","13","5","/","+"]`
- **Output:** `6`

### Example 3
- **Input:** `tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]`
- **Output:** `22`

## Constraints

- `1 <= tokens.length <= 10^4`
- `tokens[i]` is either an operator or an integer in the range `[-200, 200]`

# Approaches

## 1. Brute Force (Re-evaluate Expression)

### Idea
- Scan tokens and evaluate whenever operator found
- Replace operands and operator with result

### Time Complexity
- `O(n^2)`

### Space Complexity
- `O(n)`

### Code (Java)
```java
class Solution {
    public int evalRPN(String[] tokens) {
        List<String> list = new ArrayList<>(Arrays.asList(tokens));

        while (list.size() > 1) {
            for (int i = 0; i < list.size(); i++) {
                String token = list.get(i);

                if (isOperator(token)) {
                    int a = Integer.parseInt(list.get(i - 2));
                    int b = Integer.parseInt(list.get(i - 1));

                    int result = calculate(a, b, token);

                    list.remove(i);
                    list.remove(i - 1);
                    list.remove(i - 2);

                    list.add(i - 2, String.valueOf(result));
                    break;
                }
            }
        }

        return Integer.parseInt(list.get(0));
    }

    private boolean isOperator(String s) {
        return s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/");
    }

    private int calculate(int a, int b, String op) {
        if (op.equals("+")) return a + b;
        if (op.equals("-")) return a - b;
        if (op.equals("*")) return a * b;
        return a / b;
    }
}
```

## 2. Stack (Optimal)

### Idea

* Push numbers to stack
* When operator found, pop two elements and apply operation
* Push result back

### Time Complexity

* `O(n)`

### Space Complexity

* `O(n)`

### Code (Java)

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();

        for (String token : tokens) {
            if (isOperator(token)) {
                int b = stack.pop();
                int a = stack.pop();

                int result = calculate(a, b, token);
                stack.push(result);
            } else {
                stack.push(Integer.parseInt(token));
            }
        }

        return stack.peek();
    }

    private boolean isOperator(String s) {
        return s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/");
    }

    private int calculate(int a, int b, String op) {
        if (op.equals("+")) return a + b;
        if (op.equals("-")) return a - b;
        if (op.equals("*")) return a * b;
        return a / b;
    }
}
```

## Visualization

```id="erpnviz"
tokens = ["2","1","+","3","*"]

Push 2, 1 → "+"
2 + 1 = 3 → push
Push 3 → "*"
3 * 3 = 9
```

## Edge Cases

* Single number
* Negative numbers
* Division truncation toward zero
* Large expressions

## Why NOT Other Approaches

### Brute Force

```java id="erpn3"
scan and replace
```

* `O(n^2)` time
* Inefficient

## Follow-up Variations

### 1. Support more operators

```java
extend calculate()
```

### 2. Infix to postfix conversion

```java id="erpn5"
use stack + precedence
```

## Conclusion

| Approach    | Time Complexity | Space Complexity | Notes      |
| ----------- | --------------- | ---------------- | ---------- |
| Brute Force | O(n^2)          | O(n)             | Not usable |
| Stack       | O(n)            | O(n)             | Optimal    |
