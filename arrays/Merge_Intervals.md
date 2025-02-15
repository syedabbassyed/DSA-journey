### **📌 Merge Intervals (LeetCode #56)**  

---

## **🔹 Problem Statement**  
Given an array of intervals where **intervals[i] = [start, end]**, merge all overlapping intervals and return an array of **non-overlapping** intervals that cover all input intervals.

---

## **🔹 My Approach (In-Place Merging, `O(1)` Space Optimization)**  
💡 **Thought Process:**  
- First, **sort the intervals** based on the start time.
- Use **two pointers (`i, j`)** to iterate and merge overlapping intervals **in-place**.
- If `intervals[i]` and `intervals[j]` overlap:
  - **Update `intervals[i]`** with the merged interval.
  - **Remove `intervals[j]`** from the list (using `.pop()`).
- This approach **modifies the input list directly**, avoiding extra space.

---

### **🔹 My Code Implementation**
```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if len(intervals) == 1:
            return intervals
        
        i, j = 0, 1
        intervals.sort()  # Sorting intervals based on the start time
        
        while j < len(intervals):
            if intervals[j][0] <= intervals[i][1]:  
                # Merge overlapping intervals
                intervals[i][1] = max(intervals[i][1], intervals[j][1])
                intervals.pop(j)  # Remove merged interval
            else:
                i += 1
                j += 1
        
        return intervals
```

### **🔹 Time & Space Complexity**
- **Sorting takes** `O(n log n)`, and merging takes `O(n)`, so the total complexity is **`O(n log n)`**.
- **Space Complexity:** `O(1)`, since merging is done **in-place**.

---

## **🔹 Optimized Approach (YouTube Solution – Using a New List, `O(n)` Space)**  
💡 **Key Optimizations:**  
- Instead of modifying the input list, we build a **new result list (`output`)**.
- **Always add non-overlapping intervals** directly to `output`.  
- **Merge intervals in `output` if necessary** by updating the last interval.

---

### **🔹 Optimized Code Implementation**
```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda pair: pair[0])  # Sort intervals by start time
        output = [intervals[0]]  # Initialize with the first interval

        for start, end in intervals:
            lastEnd = output[-1][1]

            if start <= lastEnd:  
                # Merge overlapping intervals
                output[-1][1] = max(lastEnd, end)
            else:
                # No overlap → Add to output
                output.append([start, end])
                
        return output
```

---

### **🔹 Time & Space Complexity (Optimized Solution)**
- **Sorting takes** `O(n log n)`, and merging takes `O(n)`, so the total complexity is **`O(n log n)`**.
- **Space Complexity:** `O(n)`, because a new list (`output`) is created.

---

## **🔹 Example Walkthrough**
### **Input**
```python
intervals = [[1,3], [2,6], [8,10], [15,18]]
```
### **Step-by-Step Execution**
| Step | Current Interval | `output` List | Action |
|------|----------------|-------------|---------|
| 1️⃣  | `[1,3]`        | `[[1,3]]`   | First interval added |
| 2️⃣  | `[2,6]`        | `[[1,6]]`   | Merged with `[1,3]` |
| 3️⃣  | `[8,10]`       | `[[1,6], [8,10]]` | No merge, added |
| 4️⃣  | `[15,18]`      | `[[1,6], [8,10], [15,18]]` | No merge, added |

### **Final Output**
```python
[[1,6], [8,10], [15,18]]
```

---

## **🔹 Key Takeaways**
### **What I Learned From My Approach**
✅ Sorting first makes merging easier.  
✅ In-place modifications **save space (`O(1)`)**, making it memory-efficient.  
✅ Deleting elements (`pop()`) inside a loop **can be inefficient (`O(n)`)**, so it needs careful handling.

### **What I Learned From the Optimized Solution**
✅ **Building a new result list (`output`) is cleaner** & avoids modifying input.  
✅ **Instead of popping elements, just update and append.**  
✅ **This approach is easier to implement in coding interviews.**  

---

### **🔹 Which Approach Is Better?**
| Approach | ✅ Pros | ❌ Cons |
|----------|--------|--------|
| **My In-Place Merging (`O(1)` space)** | - Saves space (modifies input) <br> - Avoids extra list creation | - Harder to read & explain <br> - `pop()` is `O(n)`, making it inefficient for large inputs |
| **YouTube Approach (`O(n)` space)** | - More readable & easy to follow <br> - Avoids modifying input <br> - No `pop()`, making it more efficient | - Uses `O(n)` extra space |

---

### **🚀 Final Verdict: Both Are Correct, But...**
✅ **My Approach (In-Place Merging)** shows **strong critical thinking** and space optimization.  
✅ **The Optimized Approach (New List)** is **easier to explain & preferred in interviews** for clarity.  

🛠 **Interview Tip:** If asked in an interview, start with the **space-efficient (in-place) approach**, then mention the **new list approach for better readability**.  