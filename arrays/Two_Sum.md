# Two Sum (LeetCode #1)

## Learning Notes

### Key Insight:
I learned how to use a **HashMap (dictionary in Python)** to solve the problem efficiently. Instead of initializing and fully populating the hash map beforehand, I iterated through the list while building the hash map dynamically.

### Approach:
- For each element in the list, calculate the difference between the `target` and the current element (`diff = target - current_element`).
- Check if `diff` already exists in the hash map.
  - If it does, it means we’ve found the two indices (the current index and the index of the `diff` stored in the hash map).
  - Return these two indices.
- If `diff` doesn’t exist in the hash map, add the current element to the hash map with its index.

---

### Example Walkthrough:
**Input:**  
`nums = [3, 2, 4]`, `target = 6`

**Steps:**
1. Start with an empty hash map: `{}`.
2. Iterate through the list:
   - **Index 0, Value 3**:  
     - `diff = target - 3 = 6 - 3 = 3`  
     - `3` is not in the hash map. Add `{3: 0}` to the hash map.  
     - Hash map now: `{3: 0}`.
   - **Index 1, Value 2**:  
     - `diff = target - 2 = 6 - 2 = 4`  
     - `4` is not in the hash map. Add `{2: 1}` to the hash map.  
     - Hash map now: `{3: 0, 2: 1}`.
   - **Index 2, Value 4**:  
     - `diff = target - 4 = 6 - 4 = 2`  
     - `2` exists in the hash map at index `1`.  
     - Return `[1, 2]` as the result.

---

### Why This Works:
The hash map helps store each number and its index as we iterate. When we find the complement (`diff`) in the hash map, it guarantees that the sum of the two numbers equals the `target`. 

- **Hash map after processing**: `{3: 0, 2: 1, 4: 2}`.
- **Return value**: `hash_map[diff]` (index of `diff`) and the current index.

---

### Benefits of This Approach:
1. **Efficiency**: 
   - The solution works in **O(n)** time because each number is processed once.
   - Space complexity is also **O(n)** due to the hash map storage.
2. **Dynamic Hash Map Building**: 
   - You don’t need to initialize the hash map with all elements beforehand.
   - It’s built incrementally as you iterate through the list.

---