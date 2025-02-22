# Valid Parentheses (LeetCode #20)

## **Problem Statement**

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['`, and `']'`, determine if the input string is valid.

An input string is valid if:
1. The brackets must close in the correct order.
2. Every opening bracket must have a corresponding closing bracket.

### **Example:**
```python
Input: s = "()"  
Output: true

Input: s = "()[]{}"  
Output: true

Input: s = "(]"  
Output: false
```

---

## **Approach and Solution**

### **First Approach (Using Stack with Edge Cases)**

#### **Explanation:**
- I used a **stack** to solve the problem, as it’s the classic way to validate parentheses.
- Added **edge cases**:
  - If the string’s length is **odd**, it's automatically invalid because it can't have matching pairs.
  - If the string starts with a **closing bracket**, it's invalid (no opening bracket to match).
- **Time Complexity**: **O(n)** (where `n` is the length of the string)  
- **Space Complexity**: **O(n/2)** (since we use a stack to hold half of the characters in the worst case)

#### **Code Implementation:**
```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 2 == 1:
            return False
        if s[0] == "}" or s[0] == "]" or s[0] == ")":
            return False
        balancerStack = list()
        top = -1
        for i in range(0, len(s)):
            if s[i] == "(" or s[i] == "[" or s[i] == "{":
                balancerStack.append(s[i])
                top += 1
            if s[i] == "}" or s[i] == "]" or s[i] == ")":
                if len(balancerStack) >= 1 and top != -1:
                    if (s[i] == "}" and balancerStack[top] == "{") or \
                       (s[i] == "]" and balancerStack[top] == "[") or \
                       (s[i] == ")" and balancerStack[top] == "("):
                        balancerStack.pop()
                        top -= 1
                    else:
                        return False
                else:
                    return False
        if len(balancerStack) == 0:
            return True
        return False
```

---

### **Optimized Approach (Using Hash Map for Comparison)**

#### **Explanation:**
- In the optimized approach, I simplified the comparison process by using a **hash map** to map each closing parenthesis to its corresponding opening parenthesis.
- The stack stores opening parentheses as usual, and the comparison of closing parentheses is done more efficiently by checking the top of the stack against the hash map.
- This approach works for all edge cases and is **more concise**.
- **Time Complexity**: **O(n)**  
- **Space Complexity**: **O(n)** (due to the stack storage)

#### **Code Implementation:**
```python
class Solution:
    def isValid(self, s: str) -> bool:
        balancerStack = list()
        balancerCheckHashMap = {
            ")" : "(",
            "}" : "{",
            "]" : "["
        }

        for i in range(0, len(s)):
            if s[i] in balancerCheckHashMap:
                if balancerStack and balancerStack[-1] == balancerCheckHashMap[s[i]]:
                    balancerStack.pop()
                else:
                    return False
            else:
                balancerStack.append(s[i])
        
        return True if not balancerStack else False
```

---

### **Example Walkthrough**

#### **Input:**
```python
s = "()[]{}"
```

#### **Processing:**
1. Start with an empty stack: `[]`.
2. Iterate through the string:
   - **Character '('**: Add to stack → `['(']`.
   - **Character ')'**: Top of stack is `'('`, match → Pop → `[]`.
   - **Character '['**: Add to stack → `['[']`.
   - **Character ']'**: Top of stack is `'['`, match → Pop → `[]`.
   - **Character '{'**: Add to stack → `['{']`.
   - **Character '}'**: Top of stack is `'{'`, match → Pop → `[]`.
3. Stack is empty, return `True`.

#### **Final Output:**
```python
True
```

---

### **Why This Works Better**

✅ **Optimized Comparison:**  
- Using the hash map makes the matching of parentheses more efficient by directly mapping closing parentheses to opening ones.  

✅ **Edge Case Handling:**  
- The optimized approach is more concise and naturally handles edge cases like mismatched pairs, empty strings, or strings starting with closing parentheses.  

---

### **Alternative Approaches (Not Ideal for Interviews)**

1. **Using a List for Stack Operations** (similar to `list()` but less efficient):
   - While this is common, `list` has performance limitations when it comes to adding/removing elements at the start or middle.

2. **Using a One-Liner Solution** (but not ideal for interviews due to limited explanation):
   ```python
   class Solution:
       def isValid(self, s: str) -> bool:
           return sorted(s) == sorted(s[::-1])
   ```

---

### **Key Takeaways from This Problem:**
- **Stack-based approach** is a classic solution to problems involving matching parentheses.  
- **Optimized comparison** using a hash map simplifies the logic, making it more efficient.  
- Learning to **refine your approach** based on a more efficient solution is an essential skill for interviews.  