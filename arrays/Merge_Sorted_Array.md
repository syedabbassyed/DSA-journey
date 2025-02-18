### **📌 Merge Sorted Array (LeetCode #88) - Problem Journey**  

---

## **🔹 Problem Statement**  
You are given two sorted integer arrays `nums1` and `nums2`, along with their respective sizes `m` and `n`.  
- `nums1` has extra space (`0`s) at the end to accommodate `nums2`.  
- You need to merge `nums2` into `nums1`, **modifying `nums1` in-place** while keeping it sorted.  

### **🔹 Example Walkthrough**  

**Example 1:**  
```python
Input: nums1 = [1,2,3,0,0,0], m = 3  
       nums2 = [2,5,6], n = 3  
Output: [1,2,2,3,5,6]
```

**Example 2:**  
```python
Input: nums1 = [1], m = 1  
       nums2 = [], n = 0  
Output: [1]
```

---

## **🔹 My First Approach (Almost Correct but Had Edge Case Issues)**  
### **🔹 Thought Process:**  
- I started by using **two pointers (`top1` and `top2`)**, one for `nums1` and one for `nums2`.  
- I wanted to **merge from the back** by placing the **largest element in `nums1` last** to avoid shifting elements manually.  
- However, my initial approach **had issues with edge cases**:  
  - **Loop condition was incorrect (`while top2 > 0` instead of `>= 0`)**, causing it to miss the last element.  
  - **Didn’t handle `top1 < 0` correctly**, which caused out-of-bounds errors.  

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        top1, top2 = m - 1, n - 1
        top = m + n -1
        while top2 > 0:  # ❌ Incorrect condition
            if nums1[top1] > nums2[top2]:  
                nums1[top] = nums1[top1]
                top1 -= 1
            else:
                nums1[top] = nums2[top2]
                top2 -= 1
            top -= 1
```

🔹 **What Went Wrong?**  
1️⃣ **Missed the last element of `nums2`** because of `while top2 > 0` instead of `>= 0`.  
2️⃣ **Didn’t check if `top1` became negative**, causing an out-of-bounds error in some test cases.  
3️⃣ **Edge cases like `m == 0` weren’t handled.**  

---

## **🔹 Optimized Solution (After Fixing Edge Cases)**  
### **🔹 Fixes Applied:**  
✅ **Used `while top2 >= 0` to ensure all elements in `nums2` are merged.**  
✅ **Added `if top1 >= 0` to avoid accessing out-of-bounds indices.**  
✅ **Ensured correct order by merging backwards from `nums1[m+n-1]`.**  

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        top1, top2 = m - 1, n - 1
        top = m + n - 1
        
        while top2 >= 0:  # ✅ Fix: Now includes last element of nums2
            if top1 >= 0 and nums1[top1] > nums2[top2]:  # ✅ Fix: Avoids accessing nums1[-1]
                nums1[top] = nums1[top1]
                top1 -= 1
            else:
                nums1[top] = nums2[top2]
                top2 -= 1
            top -= 1
```

---

## **🔹 Time & Space Complexity**  
✅ **Time Complexity:** `O(n + m)` → **Single pass through both arrays.**  
✅ **Space Complexity:** `O(1)` → **In-place merging, no extra space used.**  

---

## **🔹 Example Walkthrough of Corrected Solution**
### **Input:**
```python
nums1 = [1,2,3,0,0,0], m = 3  
nums2 = [2,5,6], n = 3  
```
**Initial Pointers:**  
```
top1 = 2  (nums1[m-1] = 3)
top2 = 2  (nums2[n-1] = 6)
top  = 5  (Last position in nums1)
```

### **Step-by-Step Execution:**
| **Step** | **nums1 (Current State)** | **Pointers** | **Action Taken** |
|------|----------------------|----------|-----------------|
| 1️⃣ | `[1,2,3,0,0,6]` | `top1=2`, `top2=1`, `top=4` | Moved `6` from `nums2` |
| 2️⃣ | `[1,2,3,0,5,6]` | `top1=2`, `top2=0`, `top=3` | Moved `5` from `nums2` |
| 3️⃣ | `[1,2,3,3,5,6]` | `top1=1`, `top2=0`, `top=2` | Moved `3` from `nums1` |
| 4️⃣ | `[1,2,2,3,5,6]` | `top1=1`, `top2=-1`, `top=1` | Moved `2` from `nums2` |
✅ **Final Output:** `[1,2,2,3,5,6]`  

---

## **🔹 Edge Cases Covered in Fixed Solution**
✅ **`nums2` is empty (`n == 0`)** → Does nothing, `nums1` remains unchanged.  
✅ **`nums1` is empty (`m == 0`)** → Copies `nums2` into `nums1`.  
✅ **Already sorted input (`nums1 = [1,2,3], nums2 = [4,5,6]`)** → Merges correctly.  
✅ **All elements in `nums2` are smaller (`nums1 = [4,5,6], nums2 = [1,2,3]`)** → Works correctly.  

---

## **🔹 My Takeaways from This Problem**
🔥 **I was very close to the correct solution, but small logical mistakes (loop conditions, edge cases) made a big difference.**  
🔥 **Merging from the back is a powerful trick for in-place array merging.**  
🔥 **It’s important to check boundary conditions (`top1 >= 0`) before accessing array elements.**  
🔥 **If I had limited time in an interview, I could have first done `nums1[:] = sorted(nums1[:m] + nums2)`, then optimized it.**  