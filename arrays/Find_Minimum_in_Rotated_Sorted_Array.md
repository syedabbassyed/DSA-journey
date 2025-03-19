### 📌 Find Minimum in Rotated Sorted Array (LeetCode 153)

#### **Problem Statement**
Given a rotated sorted array `nums`, find the minimum element. The array was originally sorted in ascending order but has been rotated at some pivot.

🔹 **Constraints:**
- You must write an **O(log n)** time complexity solution.

---

## **📝 My Approach (First Attempt on March 18, 2025)**
I attempted to solve the problem after a long break from DSA. My brain felt rusty, and I struggled to come up with the approach. I had forgotten how **binary search in rotated arrays** worked. 😭

### **Mistakes in My Thought Process**
- I initially thought of a brute-force solution, scanning the entire array, but that would be **O(n)**.
- I then realized that the pivot (smallest element) is always in the **unsorted part** of the rotated array.
- I didn't immediately remember how to efficiently adjust `left` and `right` in **binary search**.

⏳ **Result:** I couldn’t solve it on my own and had to **understand the solution first**.

---

## **🚀 Optimized Approach**
Once I understood the logic, I reattempted it the next day (March 19, 2025) and solved it **under a minute!** 🎉

### **Final Code (Solved in <1 min)**
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if nums[mid] > nums[right]:  # Pivot is in the right half
                left = mid + 1
            else:  # Pivot is in the left half (including mid)
                right = mid
        
        return nums[left]
```

### **🧐 Thought Process**
1. **Identify the sorted & rotated parts**:
   - If `nums[mid] > nums[right]`, it means the **pivot is on the right side**.
   - Otherwise, it's on the left side.
2. **Use Binary Search**:
   - Keep shrinking the search space.
   - When `left == right`, we've found the minimum.

⏳ **Time Complexity:** **O(log n)**  
💾 **Space Complexity:** **O(1)**  

---

## **✅ Key Takeaways**
- **Binary Search in Rotated Arrays**: Instead of brute force, focus on the **sorted vs. rotated part**.
- **Reattempting Helped!**: On March 18, I was stuck. But on March 19, I solved it instantly.
- **Don't Panic After a Long Break**: The brain might feel rusty, but **patterns come back fast!**

---