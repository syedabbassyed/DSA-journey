# **Longest Repeating Character Replacement (LeetCode #424)**  

## **Problem Statement**  
Given a string `s` and an integer `k`, you can replace at most `k` characters in the string to make a substring with all the same characters.  

Return the length of the **longest possible substring** that can be obtained.  

### **Example:**  
```python
Input: s = "AABABBA", k = 1  
Output: 4  
Explanation: Replace the second 'B' with 'A' → "AAAABBA", longest repeating substring = "AAAA" (length 4).  

Input: s = "ABAB", k = 2  
Output: 4  
Explanation: Replace both 'B's with 'A' → "AAAA", longest repeating substring = "AAAA" (length 4).
```

---

## **Approach and Solution**  

### **First Attempt (Incorrect)**
#### **Explanation:**  
- I used **two pointers (`left` and `right`)** to extend the substring.  
- **Problem:** The logic didn't correctly track replacements and failed on `k = 0` cases.  

#### **Code Implementation:**  
```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        
        left = 0
        longestSubStrLength = 1
        for right in range(1, len(s)):
            longestSubStrLength += 1
            if s[left] != s[right]:
                if k > 0:
                    k -= 1
                else:
                    longestSubStrLength -= 1
            if k == 0:
                if right + 1 != len(s) and s[left] == s[right + 1]:
                    longestSubStrLength += 1
                break
        
        return longestSubStrLength
```
#### **Why This Failed?**  
❌ **Didn't correctly track replacements** → It modified `k` directly instead of managing a window.  
❌ **Failed edge cases (`k = 0, s = "AAAA"`)**.  
❌ **Didn't account for variable window sizes dynamically**.  

---

### **Optimized Approach (Sliding Window + Frequency Map)**
#### **Key Idea:**  
- **Maintain a sliding window** (`left` and `right` pointers).  
- Use a **hashmap (`OccurrencesMap`)** to track character frequencies.  
- The **window is valid as long as**:
  - `window size - most frequent character count ≤ k`  
- **If the condition is violated**, shrink the window (`left += 1`).  

#### **Time Complexity:**  
- **O(n)** → Each character is processed at most twice (once when expanding, once when shrinking).  

#### **Code Implementation:**  
```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        longestSubStrLength = 0
        OccurrencesMap = {}

        left = 0

        for right in range(0, len(s)):
            OccurrencesMap[s[right]] = 1 + OccurrencesMap.get(s[right], 0)

            currentWindowLength = right - left + 1

            if currentWindowLength - max(OccurrencesMap.values()) > k:
                OccurrencesMap[s[left]] -= 1
                left += 1
                currentWindowLength = right - left + 1
                
            longestSubStrLength = max(currentWindowLength, longestSubStrLength)

        return longestSubStrLength
```

✅ **Why This Works Better?**  
- Uses **a frequency map** instead of modifying `k` directly.  
- **Window shrinks only when needed**, improving efficiency.  
- **Works for all edge cases (`k = 0, s = "AAAA"`)**.  

---

### **Further Optimized Approach (Tracking Max Frequency Separately)**
#### **Key Idea:**  
- Instead of recomputing `max(OccurrencesMap.values())` every time, maintain a variable `highestFrequency`.  
- **Improvement:** **Lookup time reduces from O(26) to O(1)**.  

#### **Code Implementation:**  
```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        longestSubStrLength = 0
        OccurrencesMap = {}

        left = 0
        highestFrequency = 0
        for right in range(0, len(s)):
            OccurrencesMap[s[right]] = 1 + OccurrencesMap.get(s[right], 0)
            highestFrequency = max(highestFrequency, OccurrencesMap[s[right]])
            currentWindowLength = right - left + 1

            if currentWindowLength - highestFrequency > k:
                OccurrencesMap[s[left]] -= 1
                left += 1
                currentWindowLength = right - left + 1
                
            longestSubStrLength = max(currentWindowLength, longestSubStrLength)

        return longestSubStrLength
```

✅ **Why This Works Even Faster?**  
- **Tracks max frequency dynamically** instead of recalculating from the hashmap.  
- **Same O(n) complexity but reduced unnecessary calculations**.  
- **Better performance on large inputs.**  

---

### **Example Walkthrough**
#### **Input:**  
```python
s = "AABABBA", k = 1
```

#### **Processing:**  
| Step | Left Pointer | Right Pointer | Frequency Map | Window Size | Longest Length |
|------|------------|--------------|---------------|-------------|----------------|
| `A`  | 0 | 0 | `{A:1}` | 1 | 1 |
| `A`  | 0 | 1 | `{A:2}` | 2 | 2 |
| `B`  | 0 | 2 | `{A:2, B:1}` | 3 | 3 |
| `A`  | 0 | 3 | `{A:3, B:1}` | 4 | 4 |
| `B`  | 0 | 4 | `{A:3, B:2}` | 5 | 4 |
| `B`  | 1 | 5 | `{A:2, B:3}` | 5 | 4 |
| `A`  | 1 | 6 | `{A:3, B:3}` | 4 | 4 |

#### **Final Output:**  
```python
4
```

---

### **Why This Problem Was Important**
✅ **Deepened understanding of Sliding Window.**  
✅ **Learned to track constraints dynamically.**  
✅ **Improved performance using max frequency tracking.**  

---

### **Key Takeaways**
- **First approach** → Incorrect due to modifying `k` directly and not handling dynamic windowing.  
- **Sliding Window with HashMap** → Efficient but required frequent max lookups.  
- **Optimized Sliding Window (Tracking Max Frequency)** → **Best solution, avoids redundant lookups.**  

This problem **reinforced my ability to manage dynamic window sizes efficiently** and strengthened **my problem-solving intuition for sliding window problems.** 🎯🔥  