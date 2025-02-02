# Valid Palindrome (LeetCode #125)

## **Problem Statement**
A string is considered a **valid palindrome** if, after removing all non-alphanumeric characters and ignoring cases, it reads the same forward and backward.

### **Example:**
```python
Input: s = "A man, a plan, a canal: Panama"
Output: True

Input: s = "race a car"
Output: False
```

---

## **Approach and Solution**

### **First Approach (Using Stack and Reversal)**
#### **Explanation:**
- I attempted to process the string by **removing non-alphabetic characters** and reversing it to check for a palindrome.
- **Issue:** My approach **removed numbers as well**, causing incorrect results for cases where numbers were part of the palindrome.

#### **Code Implementation:**
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        processedStringStack = []

        for i in range(0, len(s)):
            correspondingAscii = ord(s[i].lower()) - ord("a") + 1
            if 1 <= correspondingAscii <= 26:  # Only letters, missing numbers
                processedStringStack.append(s[i].lower())
        
        return processedStringStack == processedStringStack[::-1]
```

#### **Why It Failed?**
- ❌ Removed **numeric characters**, causing incorrect results.
- ❌ Time Complexity: **O(n)** (string traversal) + **O(n)** (reversing) = **O(n)**.

---

### **Second Approach (Two Pointers)**
#### **Explanation:**
- Used **two pointers** (`left` and `right`) to compare characters from both ends.
- Skipped non-alphanumeric characters using conditional checks.
- **Issue:** I **didn’t properly skip characters** before comparison, causing index errors.

#### **Code Implementation:**
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        while left < right:
            if not (('0' <= s[left] <= '9') or ('A' <= s[left] <= 'Z') or ('a' <= s[left] <= 'z')):
                left += 1
            if not (('0' <= s[right] <= '9') or ('A' <= s[right] <= 'Z') or ('a' <= s[right] <= 'z')):
                right -= 1
            if s[left].lower() != s[right].lower():
                return False
            left += 1
            right -= 1
        return True
```

#### **Why It Failed?**
- ❌ **Did not properly handle non-alphanumeric skips** (needed a `while` loop for skipping).
- ❌ Could cause **index errors** when skipping consecutive non-alphanumeric characters.

---

### **Third Approach (Nested While for Skipping Non-Alphanumeric Characters)**
#### **Explanation:**
- **Fixed the issue** by adding **nested while loops** to properly skip non-alphanumeric characters **before comparison**.
- This prevents **index errors** and ensures correct palindrome validation.

#### **Code Implementation:**
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        while left < right:
            while not ((ord('0') <= ord(s[left]) <= ord('9')) or (ord('A') <= ord(s[left]) <= ord('Z')) or (ord('a') <= ord(s[left]) <= ord('z'))) and left < right:
                left += 1
            while not ((ord('0') <= ord(s[right]) <= ord('9')) or (ord('A') <= ord(s[right]) <= ord('Z')) or (ord('a') <= ord(s[right]) <= ord('z'))) and left < right:
                right -= 1
            if s[left].lower() != s[right].lower():
                return False
            left += 1
            right -= 1
        return True
```

#### **Why It Works?**
✅ **Properly skips non-alphanumeric characters** before comparison.  
✅ **Uses two-pointer technique**, reducing space usage.  
✅ **Time Complexity:** **O(n)** (each character is processed once).  
✅ **Space Complexity:** **O(1)** (only uses pointers, no extra storage).  

---

### **Optimized Approach (Using a Helper Function)**
#### **Key Idea:**
- Instead of writing long conditions for checking **if a character is alphanumeric**, we use a **helper function**.
- This improves **readability and maintainability**.

#### **Code Implementation:**
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        while left < right:
            while not self.isAlphaNum(s[left]) and left < right:
                left += 1
            while not self.isAlphaNum(s[right]) and left < right:
                right -= 1
            if s[left].lower() != s[right].lower():
                return False
            left += 1
            right -= 1
        return True

    def isAlphaNum(self, ch):
        return ((ord('0') <= ord(ch) <= ord('9')) or 
                (ord('A') <= ord(ch) <= ord('Z')) or 
                (ord('a') <= ord(ch) <= ord('z')))
```

#### **Why It Works?**
✅ **Helper function improves readability.**  
✅ **Same O(n) time complexity but cleaner implementation.**  
✅ **Preferred approach in interviews due to clarity.**  

---

### **Example Walkthrough**
#### **Input:**
```python
s = "A man, a plan, a canal: Panama"
```

#### **Processing:**
- Remove non-alphanumeric characters → `"amanaplanacanalpanama"`
- Compare first and last characters:  
  - `'a' == 'a'` ✅  
  - `'m' == 'm'` ✅  
  - `'a' == 'a'` ✅  
  - … (continues until completion)

#### **Final Output:**
```python
True
```

---

### **Why This Problem Matters**
✅ **Introduces the Two-Pointer technique**.  
✅ **Helps in understanding how to filter out unwanted characters efficiently**.  
✅ **Improves ability to handle edge cases in string processing problems**.  

---

### **Final Takeaways**
- **First approach (stack + reverse)** was incorrect because it removed numeric characters.  
- **Second approach (two pointers)** was close but had skipping issues.  
- **Third approach (nested while loops)** fixed skipping issues and ensured correctness.  
- **Optimized approach (helper function)** improved readability while maintaining efficiency.  

This problem helped **refine my understanding of two-pointers, character filtering, and edge case handling.** 🎯🔥  