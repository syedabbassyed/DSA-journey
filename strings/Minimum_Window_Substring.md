# **Minimum Window Substring (LeetCode #76 - Hard)**  

## **Problem Statement**  
Given two strings `s` and `t`, return the **smallest substring in `s`** that contains all characters of `t`. If no such substring exists, return an empty string `""`.  

### **Example:**  
```python
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"

Input: s = "a", t = "a"
Output: "a"

Input: s = "a", t = "aa"
Output: ""
```

---

## **Approach and Solution**  

### **🚀 First Attempt (Brute Force - Failed)**
#### **Idea:**  
- Store **all possible substrings** containing `t`, then find the smallest one.  

#### **Issues with This Approach:**  
❌ **O(n²) Time Complexity** → Checking all substrings is too slow for large inputs.  
❌ **Didn’t correctly track character frequencies** → Only stored unique characters.  
❌ **Didn’t handle edge cases properly.**  

#### **Code Implementation:**
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        frequencyMapT = {}

        for i in range(len(t)):
            frequencyMapT[t[i]] = 1 + frequencyMapT.get(t[i], 0)
        
        left = 0
        newSubstrArray = []
        visited = set()
        for right in range(len(s)):
            if s[right] in frequencyMapT:
                visited.add(s[right])
            if len(visited) == len(t):
                newSubstrArray.append(s[left: right + 1])
                left = right
                visited = set()
        print(newSubstrArray)
```

---

### **🔄 Second Attempt (Tried Moving Pointers - Got Stuck)**
#### **Idea:**  
- Use two pointers (`left`, `right`) to find a valid substring.  
- Track characters with a **set** instead of a **hash map**.  

#### **Issues with This Approach:**  
❌ **Used `set()` instead of frequency count** → Didn’t track duplicates properly.  
❌ **Incorrect shrinking logic** → Couldn’t correctly move `left`.  
❌ **Got stuck handling window expansion/contraction.**  

#### **Code Implementation:**
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        frequencyMapT = {}

        for i in range(len(t)):
            frequencyMapT[t[i]] = 1 + frequencyMapT.get(t[i], 0)
        
        minimumWindow = float('inf')

        left = 0
        visited = set()
        secondOccurrence = 0
        minimumWindowLength = len(s)
        minimumWindowSubstr = ""
        for right in range(len(s)): 
            if s[right] in frequencyMapT:
                visited.add(s[right])
            if len(visited) == 2:
                secondOccurrence = right
            if len(visited) == len(s):
                if minimumWindowLength > (right - left + 1):
                    minimumWindowLength = right - left + 1
                    minimumWindowSubstr = s[left: right + 1]
                    left = secondOccurrence

                    visited = set()
                    for i in range(left, right + 1):
                        if s[i] in frequencyMapT:
                            visited.add(s[i])
                        if len(visited) == 2:
                            secondOccurrence = right
```

---

### **✅ Final Approach (Sliding Window - Optimized)**
#### **Key Idea:**  
- **Sliding Window + HashMap** to efficiently track required characters.  
- Expand `right` to find a valid window, then shrink `left` to minimize it.  
- Maintain a **`formed`** counter to track when we have a valid window.  

#### **Time Complexity:**  
- **O(n)** → Each character is processed **at most twice** (once by `right`, once by `left`).  
- **Space Complexity: O(1)** (since we only store 26 characters in the hash map).  

#### **Code Implementation:**
```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        frequencyMapT = {}

        for i in range(len(t)):
            frequencyMapT[t[i]] = 1 + frequencyMapT.get(t[i], 0)
        
        minimumWindow = float('inf')
        minimumWindowSubstr = ""
        left = 0
        right = 0
        formed = 0
        windowCounts = {}
        required = len(frequencyMapT)

        while right < len(s):
            windowCounts[s[right]] = 1 + windowCounts.get(s[right], 0)

            if s[right] in frequencyMapT and windowCounts[s[right]] == frequencyMapT[s[right]]:
                formed += 1

            while formed == len(frequencyMapT):
                if (right - left + 1) < minimumWindow:
                    minimumWindow = (right - left + 1)
                    minimumWindowSubstr = s[left : right + 1]

                windowCounts[s[left]] -= 1

                if s[left] in frequencyMapT and windowCounts[s[left]] < frequencyMapT[s[left]]:
                    formed -= 1
                left += 1
            right += 1
        
        return minimumWindowSubstr
```

---

### **🔑 Key Takeaways**
- **First Approach:** Brute-force storing substrings **(Incorrect)**
- **Second Approach:** Tried two pointers but got stuck **(Better but incorrect)**
- **Final Approach:** **Optimized Sliding Window (Correct & Efficient)**  