# **Find the Index of the First Occurrence in a String** (LeetCode #28)

## **Problem Statement**
Given two strings `haystack` and `needle`, return the **index of the first occurrence** of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.  

### **Example:**
```python
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6, but we return 0 (first occurrence).

Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" is not a substring of "leetcode".
```

---

## **Approach and Solution**  

### **First Attempt (Failed to Solve Completely)**
#### **Explanation:**
- I attempted to track occurrences of `needle` within `haystack` by **building a substring dynamically (`Visited`)**.
- Used **left and right pointers**, but the logic failed when handling edge cases.  

#### **Issues with This Approach:**
❌ **Didn't handle substring matching correctly**.  
❌ **Unnecessary tracking of characters manually**, making the logic complex.  

#### **Code Implementation:**
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if len(haystack) == len(needle) and haystack == needle:
            return 0
        left = 0
        Visited = ""
        for right in range(len(haystack)):
            
            if haystack[right] in needle:
                Visited += haystack[right]
            else:
                if len(Visited) == len(needle):
                    if Visited == needle:
                        return left
                else:
                    left = right + 1
                    Visited = ""
            if right == len(haystack) - 1:
                if len(Visited) == len(needle):
                    if Visited == needle:
                        return left
                else:
                    if haystack[right] == needle:
                        return right
            print(Visited)
        
        return -1
```
⛔ **Why This Failed?**
- The logic **did not correctly track substring formation**.
- **Overcomplicated approach** with unnecessary tracking of `Visited`.

---

### **Second Approach (Basic Substring Matching)**
#### **Explanation:**
- Iterate through `haystack` and check for a **match starting at each character**.
- If a match is found, return the **starting index**.
- **Time Complexity:** **O(n * m)** (where `n` is `haystack` length and `m` is `needle` length).

#### **Code Implementation:**
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if needle == "":
            return 0
        
        for i in range(len(haystack)):
            if haystack[i] == needle[0]:  # Check if the first character matches
                if haystack[i: len(needle) + i] == needle:
                    return i
        
        return -1
```
✅ **Improved Accuracy** but still inefficient.  

---

### **Optimized Approach (Sliding Window)**
#### **Key Idea:**
- Instead of checking every character individually, **only consider substrings of length `len(needle)`**.
- **Use slicing efficiently** to compare substrings.

#### **Code Implementation:**
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if needle == "":
            return 0
        
        for i in range(len(haystack) + 1 - len(needle)):  # Ensure valid range
            if haystack[i: len(needle) + i] == needle: 
                return i 
        
        return -1
```

---

### **Example Walkthrough**
#### **Input:**
```python
haystack = "hello", needle = "ll"
```

#### **Processing:**
| Index `i` | Substring `haystack[i: i + len(needle)]` | Matches `needle = "ll"`? |
|-----------|---------------------------------|----------------------|
| 0         | `"he"`                          | ❌ No                 |
| 1         | `"el"`                          | ❌ No                 |
| 2         | `"ll"`                          | ✅ Yes → return `2`   |

#### **Final Output:**
```python
2
```

---

### **Why This Works Best**
✅ **Only checks substrings of the correct length** → avoids unnecessary comparisons.  
✅ **Sliding Window approach keeps complexity O(n)** (instead of O(n * m)).  
✅ **Simpler and more readable** than earlier attempts.  

---

### **Key Takeaways**
- **First approach (manual tracking)** was inefficient and overcomplicated.  
- **Basic substring checking (O(n * m))** was an improvement but still slow.  
- **Optimized Sliding Window (O(n))** gave the best performance.  

**This problem reinforced my understanding of substring searching & efficient string traversal.** 🎯🔥  