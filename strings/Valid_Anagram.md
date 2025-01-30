Here’s your Markdown file for the **Valid Anagram** problem:

---

# Valid Anagram (LeetCode #242)

## **Learning Notes**

### **Key Insight:**
I learned how to compare two strings efficiently using **HashMaps (Dictionaries in Python)** instead of brute-force comparisons. This problem strengthens my understanding of frequency counting and different approaches to solving character-based problems.

---

## **Approach 1: Using Two HashMaps (Dictionary)**  
**Idea:**  
- Create two hash maps, one for each string, since I've been using this for Array problems.
- Count the frequency of each character in both strings.
- Compare the two hash maps.

### **Code:**
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        hashMapOfS = {}
        hashMapofT = {}

        for val in list(s):
            if val in hashMapOfS:
                hashMapOfS[val] += 1
            else:
                hashMapOfS[val] = 1

        for val in list(t):
            if val in hashMapofT:
                hashMapofT[val] += 1
            else:
                hashMapofT[val] = 1

        if len(hashMapOfS.keys()) != len(hashMapofT.keys()): #if the hashmaps have different length, then it is not an anagram
            return False

        for key in hashMapOfS:
            if hashMapofT.get(key, 0) != hashMapOfS.get(key, 0):
                return False
                
        return True
```
### **Time Complexity:** `O(n)`
### **Space Complexity:** `O(n)`

---

## **Approach 2: Optimized HashMap (Using Single Pass Frequency Count)**
**Idea:**  
- In this approach, I learnt that we can check the length and determine if it's not a palindrome.
- Also, After learning this approach, I learnt that I had a lot refactoring to do.
- Use **only one loop** to build character frequency maps for both strings simultaneously.
- Compare the frequency maps at the end.

### **Code:**
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)

        for c in countS:
            if countS[c] != countT.get(c, 0):
                return False

        return True
```
### **Time Complexity:** `O(n)`
### **Space Complexity:** `O(n)`

---

## **One-Liner Solutions (Not Ideal for Interviews)**
1. **Using `Counter` from `collections`**
```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```
2. **Using Sorting**
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
```
### **Time Complexity:** `O(n log n)` (due to sorting)  
### **Space Complexity:** `O(1)` (if sorting is done in-place) or `O(n)` (if a new sorted list is created)

---

## **Key Takeaways**
- **Hash Maps/Dictionaries** provide an `O(n)` solution, making them the best approach for large inputs.
- **Sorting** works but is less efficient (`O(n log n)`).
- **Built-in functions like `Counter` are useful but may not be preferred in interviews.**
- This problem improves my ability to use **hash maps for frequency counting**, a key pattern in many coding problems.
