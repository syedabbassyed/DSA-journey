### **📂 Trapping Rain Water (LeetCode #42, Hard)**  

---

## **📝 Problem Statement**
Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute **how much water it can trap after raining**.

---

### **Example**
#### **Input:**
```python
height = [0,1,0,2,1,0,1,3,2,1,2,1]
```
#### **Output:**
```python
6
```
#### **Explanation:**  
The total trapped water is **6 units** (as visualized in a histogram).  

---

## **💡 Approach 1: Monotonic Stack (O(n), O(n) Space)**
### **🔹 Thought Process**
- Water can only be trapped **between two higher bars**.
- A **stack can help track previous lower elevations** while iterating through heights.
- Whenever a **higher elevation is found**, we **pop from the stack** and calculate trapped water.

---

### **✅ My First Approach (Monotonic Stack)**
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        stack = []
        totalWaterTrapped = 0

        for index, elevation in enumerate(height):
            while stack and stack[-1][1] < elevation:
                lastIndex, lastElevation = stack.pop()
                if stack:
                    leftIndex, leftElevation = stack[-1]
                    possibleHeight = min(leftElevation, elevation) - lastElevation
                    width = index - leftIndex - 1
                    totalWaterTrapped += possibleHeight * width
            stack.append([index, elevation])
        
        return totalWaterTrapped
```
---

### **🔴 Issues in Monotonic Stack Approach**
1️⃣ **Uses extra space (O(n))** for the stack.  
2️⃣ **Iterates multiple times per element**, leading to slightly higher overhead.  

---

## **✅ Optimized Approach: Two Pointers (O(n), O(1) Space)**
### **🔹 Thought Process**
- Instead of using a stack, we use **two pointers (`left`, `right`)** that track the water trapping boundaries.
- We **only trap water where the left max and right max are higher than the current bar**.
- **Update left/right max dynamically** to determine how much water is stored.

---

### **🚀 Second Approach (First Attempt - Incorrect)**
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        totalWaterTrapped = 0
        left, right = 0, len(height) - 1

        leftMax, rightMax = height[left], height[right]

        while left < right:
            if height[left] < height[right]:
                if height[left] < leftMax:
                    totalWaterTrapped += leftMax - height[left]
                    left += 1  
                else:
                    leftMax = height[left]
            else:
                if height[right] < rightMax:
                    totalWaterTrapped += rightMax - height[right]
                    right -= 1
                else:
                    rightMax = height[right]
                
        return totalWaterTrapped
```
---
### **🔴 Issue in First Attempt**
- The **`left += 1` and `right -= 1` were inside the wrong condition**, causing incorrect water calculations.  
- Needed to adjust **when to move the left and right pointers** to ensure correct water trapping.

---

### **✅ Final Corrected Approach**
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        totalWaterTrapped = 0
        left, right = 0, len(height) - 1

        leftMax, rightMax = height[left], height[right]

        while left < right:
            if height[left] < height[right]:
                if height[left] < leftMax:
                    totalWaterTrapped += leftMax - height[left]
                else:
                    leftMax = height[left]
                left += 1
            else:
                if height[right] < rightMax:
                    totalWaterTrapped += rightMax - height[right]
                else:
                    rightMax = height[right]
                right -= 1
        
        return totalWaterTrapped
```
---

## **📌 Explanation of Optimized Two Pointers Approach**
1️⃣ **Maintain two pointers (`left` and `right`)**, moving towards each other.  
2️⃣ Track **leftMax and rightMax**, which define the water boundaries.  
3️⃣ **Move the pointer with the smaller height**, because only a **higher boundary on that side** will trap water.  
4️⃣ **If height[left] < leftMax**, water is stored (`leftMax - height[left]`).  
5️⃣ **Update `leftMax` or `rightMax` dynamically** to adjust to new height boundaries.  
6️⃣ **Continue until `left` meets `right`**, ensuring all trapped water is counted.

---

## **🕒 Time & Space Complexity**
- **Time Complexity:** `O(n)`, since we iterate through `height[]` once.  
- **Space Complexity:** `O(1)`, as we only use a few extra variables (`leftMax`, `rightMax`, `left`, `right`).  

---

## **🔑 Key Takeaways**
- **Monotonic Stack** helps in problems where we need to **track previous heights dynamically**.
- **Two Pointers Approach** reduces space complexity by **keeping track of boundaries instead of storing values**.
- **Choosing the optimal approach matters**—Monotonic Stack (`O(n), O(n)`) vs. Two Pointers (`O(n), O(1)`)!

---

## **🔥 Summary**
This problem teaches **efficient boundary tracking** using both **stacks and two pointers**.  
**Two Pointers** is often the best choice **when left and right boundaries matter**.

---