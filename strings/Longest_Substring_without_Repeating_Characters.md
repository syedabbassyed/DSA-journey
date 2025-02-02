# Longest Substring Without Repeating Characters (LeetCode #3)

## **Problem Statement**  
Given a string `s`, find the length of the longest substring without repeating characters.

### **Example:**
```python
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
```

---

## **Approach and Solution**  

### **First Approach (Brute Force with Nested Loops)**  
#### **Explanation:**
- Generate **all possible substrings**.
- Store substrings that **do not have repeating characters**.
- Iterate through the stored substrings and **find the longest one**.
- **Issue:** This works for small inputs but fails for large inputs due to **high time complexity**.

#### **Time Complexity:**
- **O(n²)** due to **nested loops** (checking each substring).  

#### **Code Implementation:**
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        possibleSubStrigs = []
        for i in range(0, len(s)):
            subStr = ""
            for j in range(i, len(s)):
                if s[j] in subStr:
                    if subStr not in possibleSubStrigs:
                        possibleSubStrigs.append(subStr)
                    subStr = s[j]
                else:
                    subStr += s[j]
                
                if j == len(s) - 1 and subStr not in possibleSubStrigs:
                    possibleSubStrigs.append(subStr)
        
        SubStringLength = 0

        for st in possibleSubStrigs:
            if len(st) > SubStringLength:
                SubStringLength = len(st)

        return SubStringLength
```

#### **Why It Failed?**
❌ **Too slow for large inputs** – Exceeded time limits for long strings.  
❌ **Redundant operations** – Storing substrings in an array instead of just tracking their lengths.  

---

### **Optimized Approach (Sliding Window Algorithm)**
#### **Key Idea:**
- Instead of checking **all substrings**, use a **sliding window** approach:
  - Use a **set (`visitedSet`)** to track seen characters.
  - Expand the window (`right` pointer) while there are **no duplicate characters**.
  - If a duplicate is found, **shrink the window (`left` pointer)** until it's valid again.
  - Keep track of the **longest substring length** dynamically.

#### **Time Complexity:**
- **O(n)** (each character is processed once using the sliding window).  
- **Space Complexity:** **O(min(n, 26))** (only stores unique characters in the set).  

#### **Code Implementation:**
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        visitedSet = set()
        left = 0
        longestSubStrLength = 0

        for right in range(len(s)):

            while s[right] in visitedSet:
                visitedSet.remove(s[left])
                left += 1
            
            windowLength = (right - left) + 1

            if windowLength > longestSubStrLength:
                longestSubStrLength = windowLength
            
            visitedSet.add(s[right])
        
        return longestSubStrLength
```

---

### **Example Walkthrough**
#### **Input:**
```python
s = "abcabcbb"
```

#### **Processing:**
| Step | Left Pointer | Right Pointer | Visited Set | Longest Length |
|------|------------|-------------|-------------|----------------|
| `a` | 0 | 0 | `{a}` | 1 |
| `b` | 0 | 1 | `{a, b}` | 2 |
| `c` | 0 | 2 | `{a, b, c}` | 3 |
| `a` (Duplicate!) | Move `left` → `1` | 3 | `{b, c, a}` | 3 |
| `b` (Duplicate!) | Move `left` → `2` | 4 | `{c, a, b}` | 3 |
| `c` (Duplicate!) | Move `left` → `3` | 5 | `{a, b, c}` | 3 |
| `b` (Duplicate!) | Move `left` → `4` | 6 | `{b}` | 3 |

#### **Final Output:**
```python
3
```

---

### **Why This Works Better**
✅ **Sliding window avoids recomputation** (no need to recheck substrings).  
✅ **Set-based lookup is O(1)**, making it **efficient**.  
✅ **Scales well for large inputs**, unlike the brute force approach.  

---

### **Key Takeaways**
- **First Approach (Brute Force)**
  - Too slow for large inputs (`O(n²)`)
  - Storing substrings was unnecessary
- **Optimized Approach (Sliding Window)**
  - **O(n) time complexity**
  - Uses a **set** for fast lookups
  - Keeps track of the **longest valid substring dynamically**  

This problem was a great introduction to the **Sliding Window** technique, which is useful for many **substring-related problems** in coding interviews! 🎯🔥  