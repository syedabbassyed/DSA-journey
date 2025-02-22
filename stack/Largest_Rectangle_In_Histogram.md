### **📂 Largest Rectangle in Histogram (LeetCode #84, Hard)**  

---

## **📝 Problem Statement**
Given an array `heights` representing the histogram's bar heights where the width of each bar is `1`, return the **area of the largest rectangle** in the histogram.

### **Example**
#### **Input:**
```python
heights = [2, 1, 5, 6, 2, 3]
```
#### **Output:**
```python
10
```
#### **Explanation:**  
The largest rectangle is formed by bars `[5,6]`, giving an area of `5 * 2 = 10`.

---

## **💡 Approach 1: My Initial Attempt (Incorrect)**
Before learning the optimized approach, I attempted the problem using a stack-based idea. However, the implementation contained logical errors.

### **❌ My First Approach (Incorrect)**
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = [0]  # Stack to keep track of indices
        maxArea = 0
        
        for i in range(1, len(heights)):
            area = 0
            if heights[stack[-1]] > heights[i]:  # Comparing heights using indices
                while heights[i] > heights[stack[-1]]:  
                    height = heights[stack.pop()]
                    width = heights[stack[-1]]  # Incorrect width calculation
                    area = width + height
                    maxArea = max(area, maxArea)
                stack.pop()
            else:
                stack.append(i)

        return maxArea
```
---

### **🔴 Issues in My First Approach**
1️⃣ **Incorrect comparison while popping:**  
   - `while heights[i] > heights[stack[-1]]:` should be `while heights[stack[-1]] > heights[i]:`  
   - The current logic pops when the new height is **greater** instead of **smaller**.  

2️⃣ **Incorrect width calculation:**  
   - `width = heights[stack[-1]]` is wrong.  
   - The width should be computed using index difference: `width = i - lastIndex`.  

3️⃣ **Didn't handle remaining heights in stack**  
   - Heights left in the stack at the end needed processing.  

---

## **✅ Approach 2: Optimized Solution Using Monotonic Stack (O(n))**
After watching **NeetCode's** explanation, I implemented the correct optimized approach from scratch.

### **🚀 Optimized Code**
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = []
        maxArea = 0
        
        for index, height in enumerate(heights):
            start = index
            while stack and stack[-1][1] > height:
                lastIndex, lastHeight = stack.pop()
                area = lastHeight * (index - lastIndex)
                maxArea = max(area, maxArea)
                start = lastIndex
            stack.append([start, height])
        
        for index, height in stack:
            area = height * (len(heights) - index)
            maxArea = max(area, maxArea)

        return maxArea
```
---

## **📌 Explanation of Optimized Code**
1️⃣ **Iterate over each bar**, keeping track of the **start index** of the rectangle.  
2️⃣ **Maintain a Monotonic Stack** (`stack = []`) storing `(start_index, height)`.  
3️⃣ **When a smaller height is found**, pop from the stack and calculate the **area** for the popped height.  
4️⃣ **Calculate max area for remaining bars in the stack after iteration ends.**  

---

## **🧠 Dry Run**
### **Input:**
```python
heights = [2, 1, 5, 6, 2, 3]
```
### **Stack Evolution:**
| Step | `heights[i]` | `stack` (Before Operation) | Action |
|------|-------------|----------------|-----------------------------|
| 0    | 2           | `[]`            | Push `[0,2]` |
| 1    | 1           | `[[0,2]]`       | Pop `2`, calculate area, push `[0,1]` |
| 2    | 5           | `[[0,1]]`       | Push `[2,5]` |
| 3    | 6           | `[[0,1], [2,5]]`| Push `[3,6]` |
| 4    | 2           | `[[0,1], [2,5], [3,6]]` | Pop `6`, `5`, calculate area, push `[2,2]` |
| 5    | 3           | `[[0,1], [2,2]]` | Push `[5,3]` |
| End  | -           | Process Remaining | Compute area for remaining bars |

✅ **Final Maximum Area:** `10`

---

## **🕒 Time & Space Complexity**
- **Time Complexity:** `O(n)` (Each element is pushed/popped once).  
- **Space Complexity:** `O(n)` (Stack stores indices).  

---

## **🔑 Key Takeaways**
- **Stack helps track height boundaries dynamically.**
- **Only pop when a smaller height is encountered.**
- **Calculate width using the difference between popped index & current index.**
- **Process remaining heights in the stack after iteration ends.**

---

## **🔥 Summary**
This problem introduces **Monotonic Stack**—an **essential pattern** for interval-based problems.  
Mastering it will help in problems like **Next Greater Element, Trapping Rain Water, and Histogram Variants**.

---
