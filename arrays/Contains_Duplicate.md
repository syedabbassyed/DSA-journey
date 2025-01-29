# Contains Duplicate (LeetCode #217)

## Learning Notes

### Key Insight:
I learned how to efficiently check if an element is repeated in a list using a **HashSet**. The key idea here is to maintain a set of seen elements and check if the current element already exists in the set. If it does, then a duplicate is present.

### Approach:
#### **First Approach** (using HashMap):
- We use a **HashMap (dictionary in Python)** to track the frequency of each element.
- Iterate through the list:
  - If the current element is already in the hash map, it means we've seen this element before, so we return `True`.
  - If the element isn't in the hash map, add it to the hash map and continue.
- If we iterate through the entire list without finding any duplicates, return `False`.

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashMap = {}
        for i in range(0, len(nums)):
            if nums[i] in hashMap:
                hashMap[nums[i]] += 1;
                if (hashMap[nums[i]] > 1):
                    return True
            else:
                hashMap[nums[i]] = 1;
        return False
```

#### **Optimized Approach** (using HashSet):
- Instead of using a HashMap, I switched to a **HashSet** (set in Python) for better performance.
- A HashSet automatically handles duplicates for us. It provides an efficient way to check if an element is already seen (since lookup in a set is **O(1)** on average).
- As we iterate through the list, we check if the element is already in the set:
  - If it is, return `True` as it’s a duplicate.
  - Otherwise, add the element to the set.
- If no duplicates are found after iterating through the list, return `False`.

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashSet = set()
        for num in nums:
            if num in hashSet:
                return True
            hashSet.add(num)
        return False
```

---

### Time and Space Complexity:

#### **First Approach (HashMap)**:
- **Time Complexity**:  
  - **O(n)** where `n` is the number of elements in the list.  
  - We only iterate through the list once, and each lookup or insertion operation in a hash map takes **O(1)** time on average.
  
- **Space Complexity**:  
  - **O(n)** due to the space required for storing the elements in the hash map. In the worst case, all elements are unique, so we store all `n` elements.

#### **Optimized Approach (HashSet)**:
- **Time Complexity**:  
  - **O(n)** where `n` is the number of elements in the list.  
  - Each insertion and lookup in a hash set is **O(1)** on average, and we only iterate through the list once.

- **Space Complexity**:  
  - **O(n)** due to the space required for storing the elements in the hash set. In the worst case, all elements are unique, so we store all `n` elements.

---

### Example Walkthrough:
**Input:**  
`nums = [1, 2, 3, 1]`

**Steps for First Approach (HashMap):**
1. Start with an empty hash map: `{}`.
2. Iterate through the list:
   - **Index 0, Value 1**:  
     - `1` is not in the hash map. Add `{1: 1}` to the hash map.  
     - Hash map now: `{1: 1}`.
   - **Index 1, Value 2**:  
     - `2` is not in the hash map. Add `{2: 1}` to the hash map.  
     - Hash map now: `{1: 1, 2: 1}`.
   - **Index 2, Value 3**:  
     - `3` is not in the hash map. Add `{3: 1}` to the hash map.  
     - Hash map now: `{1: 1, 2: 1, 3: 1}`.
   - **Index 3, Value 1**:  
     - `1` exists in the hash map, return `True`.

**Steps for Optimized Approach (HashSet):**
1. Start with an empty set: `{}`.
2. Iterate through the list:
   - **Index 0, Value 1**:  
     - `1` is not in the set. Add `1` to the set.  
     - Set now: `{1}`.
   - **Index 1, Value 2**:  
     - `2` is not in the set. Add `2` to the set.  
     - Set now: `{1, 2}`.
   - **Index 2, Value 3**:  
     - `3` is not in the set. Add `3` to the set.  
     - Set now: `{1, 2, 3}`.
   - **Index 3, Value 1**:  
     - `1` is already in the set, return `True`.

---

### Why This Works:
- **First Approach (HashMap)**: The hash map stores each number, and if a number repeats, it increments its count. If the count exceeds `1`, it indicates a duplicate, and we return `True`.
- **Optimized Approach (HashSet)**: The set stores unique elements. If an element is encountered again, it indicates a duplicate, so we immediately return `True`.

---

### Benefits of the Optimized Approach:
1. **Efficiency**:
   - The optimized solution runs in **O(n)** time, where `n` is the length of the list, because each insertion and lookup in a hash set is **O(1)** on average.
   - The space complexity is **O(n)** due to the hash set storing each unique element in the list.
2. **Cleaner Code**: Using a set simplifies the logic as we no longer need to track counts; we just check if the element already exists in the set.
3. **Better Performance**: The optimized approach is more efficient since it doesn’t require the additional operations involved in maintaining a hash map with counts.

---

### Final Thoughts:
- The optimized approach using a **HashSet** is both **simpler** and **more efficient**.
- Using a **HashSet** reduces unnecessary complexity and improves performance, making it the better solution for this problem.