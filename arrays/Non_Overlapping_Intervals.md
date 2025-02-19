### **📌 Non-Overlapping Intervals (LeetCode #435) - Problem Journey**  

---

## **🔹 Problem Statement**  
Given an array of intervals `intervals[i] = [start_i, end_i]`,  
you need to **remove the minimum number of intervals** to ensure that **no two intervals overlap**.  

👉 **Return the minimum number of intervals that need to be removed.**  

---

## **🔹 Example Walkthrough**  

### **Example 1:**  
```python
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]  
Output: 1  
```
**Explanation:**  
- We can remove `[1,3]`, so the remaining intervals `[1,2], [2,3], [3,4]` don’t overlap.  

---

### **Example 2:**  
```python
Input: intervals = [[1,2],[1,2],[1,2]]  
Output: 2  
```
**Explanation:**  
- We must remove two intervals so that only one remains.  

---

## **🔹 My First Approach (Close but Not Fully Correct)**  

### **Thought Process:**  
- **Sort the intervals** → Sorting should help us determine the order in which overlaps occur.  
- **Use Two Pointers (`i, j`)** to iterate and compare the intervals.  
- **Count the overlapping intervals** and remove them.  

### **My Code:**
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        count = 0
        intervals.sort(key=lambda x: x[1])  # ✅ Step 1: Sort by end time
        print(intervals)
        i, j = 0, 1
        length = len(intervals)

        while j < length:
            if (intervals[j][0] < intervals[i][1]) and i < length:
                count += 1  # ❌ Overlap detected, remove interval
            else:
                i += 1  # ✅ Move forward if no overlap
            j += 1

        return count
```
---

## **🔹 Issues in My Approach & Why It Was Wrong**  
🔴 **Mistake:**  
- I was **not updating `prev_end` (the last interval's end time) correctly**.  
- I was **not keeping track of which interval should be kept vs. removed.**  
- I was **not correctly handling cases where `i` should NOT be moved.**  

---

## **🔹 Optimized Solution (After Learning the Correct Approach)**  

### **Final Approach:**  
1️⃣ **Sort Intervals by their `end` time** (to remove the minimum number of intervals).  
2️⃣ **Use a `prev_end` variable** to track the last interval that was kept.  
3️⃣ **If an interval overlaps, remove it** (increment count).  
4️⃣ **If no overlap, update `prev_end` to the current interval’s `end` time.**  

### **Optimized Code:**
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0

        intervals.sort(key=lambda x: x[1])  # ✅ Step 1: Sort by end time
        count = 0
        prev_end = intervals[0][1]  # ✅ Step 2: Keep track of last non-overlapping interval

        for i in range(1, len(intervals)):  # ✅ Step 3: Iterate through the intervals
            if intervals[i][0] < prev_end:  
                count += 1  # ❌ Overlapping interval → Remove it
            else:
                prev_end = intervals[i][1]  # ✅ Non-overlapping → Update last valid end time

        return count  # ✅ Step 4: Return total removals
```

---

## **🔹 Example Walkthrough of Optimized Solution**
### **Example Input:**
```python
intervals = [[1,3], [2,3], [3,4], [1,2]]
```
### **Sorting Step:**
```python
intervals = [[1,2], [2,3], [1,3], [3,4]]  # Sorted by end time
```
### **Tracking Overlaps:**
| Step | Current Interval | Last Valid Interval (`prev_end`) | Overlap? | Action Taken | `count` (Removals) |
|------|-----------------|----------------------|----------|--------------|-------|
| 1️⃣  | `[1,2]`         | `2`                  | No       | Keep it      | 0     |
| 2️⃣  | `[2,3]`         | `3`                  | No       | Keep it      | 0     |
| 3️⃣  | `[1,3]`         | `3`                  | Yes      | Remove it    | 1     |
| 4️⃣  | `[3,4]`         | `4`                  | No       | Keep it      | 1     |

✅ **Final Output:** `1` (1 interval removed)

---

## **🔹 Time & Space Complexity**
✅ **Time Complexity:** `O(n log n)` (Sorting) + `O(n)` (One Pass) → **O(n log n)**  
✅ **Space Complexity:** `O(1)` (In-place sorting, no extra memory)  

---

## **🔹 Key Takeaways from This Problem**
✅ **Sorting by `end` time is a powerful greedy strategy for interval problems.**  
✅ **Tracking the `prev_end` interval is key to deciding which intervals to remove.**  
✅ **Always think about “which one should be removed” rather than just counting overlaps.**  
✅ **If you struggle, step through an example manually before coding.**  