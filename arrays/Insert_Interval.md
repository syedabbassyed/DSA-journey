### **📌 Insert Interval (LeetCode #57)**  

---

## **🔹 Problem Statement**  
You are given an array of **non-overlapping** intervals `intervals`, where `intervals[i] = [start_i, end_i]` represent the start and end of the interval.  

You are also given another interval `newInterval = [start, end]`, and you need to insert it into the correct position in `intervals` **while maintaining order** and **merging if necessary**.  

---

## **🔹 My First Approach (Didn’t Work)**
### **🔹 Thought Process:**  
1. Start with the **first interval in `newArray`**.  
2. Try to **merge `newInterval` if it overlaps** with the current interval.  
3. If no overlap, insert it at the correct position.  
4. Continue merging any remaining overlapping intervals.  

🔹 **Why This Didn’t Work?**  
❌ Fails when the new interval needs to be added **before any existing interval**.  
❌ Edge cases where the new interval **spans across multiple existing intervals**.  

```python
from typing import List

class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        newArray = [intervals[0]]

        isNewIntervalAdded = False
        for start, end in intervals:
            lastEnd = newArray[-1][1]
            if not isNewIntervalAdded:
                isNewIntervalAdded = True
                if newInterval[0] <= lastEnd:
                    newArray[-1][0] = min(newInterval[0], newArray[-1][0])
                    newArray[-1][1] = max(newInterval[1], lastEnd)
                else:
                    newArray.append(newInterval)
            lastEnd = newArray[-1][1]
            if start <= lastEnd:
                newArray[-1][1] = max(end, lastEnd)
            else:
                newArray.append([start, end])
            
        return newArray
```

---

## **🔹 Optimized Approach (YouTube Solution)**
### **🔹 Thought Process:**  
1. **Iterate through all intervals** and determine:  
   - If `newInterval` should be inserted **before the current interval** → Append it and return the final result.  
   - If `newInterval` should be inserted **after the current interval** → Append the interval to the result as is.  
   - If it **overlaps**, **merge it by updating `newInterval`**.  
2. **At the end, if `newInterval` is not added, append it**.  

✅ **This solution ensures O(n) time complexity and correct merging.**  

```python
from typing import List

class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        newArray = []

        for i in range(len(intervals)):
            if newInterval[1] < intervals[i][0]:  
                # If newInterval comes before the current interval, insert it and return the result
                newArray.append(newInterval)
                return newArray + intervals[i:]
            elif newInterval[0] > intervals[i][1]:  
                # If newInterval comes after the current interval, add the current interval as is
                newArray.append(intervals[i])
            else:  
                # If newInterval overlaps with current interval, merge them
                newInterval = [min(newInterval[0], intervals[i][0]), max(newInterval[1], intervals[i][1])]
            
        newArray.append(newInterval)  # Add the merged interval at the end if it wasn't inserted earlier
        return newArray
```

---

## **🔹 Time & Space Complexity**  
✅ **Time Complexity:** `O(n)` (Iterate through `intervals` once)  
✅ **Space Complexity:** `O(n)` (Worst case, when all intervals are non-overlapping and stored in `newArray`)  

---

## **🔹 Example Walkthrough**
### **Example 1:**  
```python
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]  
Output: [[1,5],[6,9]]
```
**Step-by-Step Execution:**  
1. First interval `[1,3]` overlaps with `[2,5]` → Merge to `[1,5]`.  
2. `[6,9]` remains unchanged.  

✅ **Final Output:** `[[1,5],[6,9]]`  

---

## **🔹 Key Takeaways**  
✅ **My approach struggled with inserting `newInterval` in the correct order** before merging.  
✅ **The optimized approach uses a single pass to insert and merge efficiently.**  
✅ **Understanding interval merging helps with problems like**:
   - [Merge Intervals](./arrays/Merge_Intervals.md)  