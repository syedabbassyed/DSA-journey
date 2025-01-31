# Group Anagrams (LeetCode #49)

## **Problem Statement**  
Given an array of strings `strs`, group the anagrams together.  
Two words are anagrams if they contain the same characters in different orders.  

### **Example:**
```python
Input: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
Output: [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

---

## **Approach and Solution**  

### **First Approach (Nested Loops with Frequency Counting)**
#### **Explanation:**  
1. Use **two loops** to compare every pair of words.
2. Create **character frequency dictionaries** for each pair.
3. If the frequencies match, the words are anagrams and belong in the same group.
4. Maintain a **visited set** to avoid duplicates.
5. **Time Complexity: O(n² \* k)** (where `n` is the number of words and `k` is the max word length).
6. **Issue:** This approach **works but is too slow** for large inputs, leading to a **Time Limit Exceeded (TLE)** error.

#### **Code Implementation:**
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        finalArray = []
        if len(strs) == 1:
            return [strs]

        visited = set()
        for i in range(len(strs)):
            if i not in visited:
                group = [strs[i]]
                for j in range(i + 1, len(strs)):
                    if len(strs[i]) == len(strs[j]) and j not in visited:
                        CountI, CountJ = {}, {}

                        for k in range(len(strs[i])):
                            CountI[strs[i][k]] = 1 + CountI.get(strs[i][k], 0)
                            CountJ[strs[j][k]] = 1 + CountJ.get(strs[j][k], 0)

                        if CountI == CountJ:
                            group.append(strs[j])
                            visited.add(j)
                finalArray.append(group)
                visited.add(i)
        return finalArray
```

---

### **Optimized Approach (Using Hashing)**
#### **Key Idea:**
Instead of **comparing every word**, use **a fixed-size array of 26 elements (for each letter 'a' to 'z')** as a **hashable key** to store character frequencies.

#### **Steps:**
1. **Initialize a hash map** (`finalGroup`) where the key is a **tuple of letter frequencies**, and the value is a list of words that match this frequency.
2. For each word:
   - Create a **count array** of size 26 initialized to zero.
   - Increment the count at the index corresponding to each letter in the word.
   - Convert this list to a tuple and use it as a **dictionary key**.
   - Append the word to the corresponding key in the dictionary.
3. **Return all grouped anagrams** as values of the dictionary.

#### **Time Complexity:**  
- **O(n \* k)** (where `n` is the number of words, and `k` is the max word length).
- **Much faster than O(n² \* k) because we avoid unnecessary pairwise comparisons.**

#### **Code Implementation:**
```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        finalGroup = defaultdict(list)

        for word in strs:
            count = [0] * 26  # A fixed-size array for character counts

            for char in word:
                count[ord(char) - ord("a")] += 1  # Convert character to index
            
            finalGroup[tuple(count)].append(word)  # Use tuple as a key

        return list(finalGroup.values())
```

---

### **Example Walkthrough**
#### **Input:**  
```python
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
```
#### **Processing:**
- `"eat"` → `[1, 0, 0, 0, 1, 0, 0, 0, 1, 0, ...]` → Stored under this key.
- `"tea"` → Same count → Added to `"eat"`'s group.
- `"tan"` → `[1, 0, 1, 0, 0, 0, 0, 0, 1, 0, ...]` → New key, new group.
- `"ate"` → Same as `"eat"` → Added to the first group.
- `"nat"` → Same as `"tan"` → Added to the second group.
- `"bat"` → New key, new group.

#### **Final Output:**
```python
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

---

## **Why This Works Better**
- **Avoids unnecessary comparisons** by **hashing words based on frequency** instead of checking every pair.  
- **Uses dictionary lookup (O(1))** instead of searching for matches (O(n)).  
- **Scales better for large inputs** compared to the brute-force nested loop approach.  

---

### **Alternative Approaches (Not Ideal for Interviews)**
1. **Sorting Each Word** → Sort each word alphabetically and use it as a dictionary key.
   ```python
   def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
       anagrams = defaultdict(list)
       for word in strs:
           anagrams["".join(sorted(word))].append(word)
       return list(anagrams.values())
   ```
   - **Time Complexity:** O(n \* k log k) (sorting each word).
   - **Better than brute-force but slower than the hash method.**

2. **Using `Counter` from Collections**
   ```python
   from collections import Counter
   def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
       anagrams = defaultdict(list)
       for word in strs:
           anagrams[frozenset(Counter(word).items())].append(word)
       return list(anagrams.values())
   ```
   - **Uses Counter but is less efficient** than the direct array approach.

---

## **Final Thoughts**
- The brute-force **O(n² \* k)** approach was intuitive but too slow.  
- The optimized **O(n \* k)** approach using **letter frequency hashing** is the best for interviews.  
- **Sorting or Counter-based approaches are valid but slower than the optimal approach.**  

---

## **Takeaways from This Problem**
- Learned **hashing with tuples** as dictionary keys.  
- Improved efficiency by using a **fixed-size frequency array instead of sorting**.  
- Recognized the importance of **choosing the right data structure** (hash map vs. nested loops).  
