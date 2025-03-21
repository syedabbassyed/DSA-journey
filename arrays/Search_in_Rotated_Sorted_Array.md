### **📂 Problem: Search in Rotated Sorted Array**
#### **LeetCode #33 | Medium**
✅ **Category:** Binary Search  
✅ **Pattern:** Modified Binary Search  
✅ **Concepts Used:** Divide & Conquer, Binary Search in Rotated Arrays  

---

## **📜 Problem Statement**
Given a sorted array that has been rotated at some pivot unknown to you beforehand, search for a target value in `O(log n)` time complexity.

If the target exists, return its index. Otherwise, return `-1`.

**Constraints:**
- You must solve it using `O(log n)` time complexity.
- All elements in `nums` are unique.
- `nums` is guaranteed to be rotated at least once.

---

## **💡 First Attempt (Incorrect)**
### **Approach**
- I attempted a standard **binary search** but failed to correctly handle the rotated portions.
- My mistake was assuming a normal sorted array, leading to incorrect `left` and `right` pointer updates.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[left] == target:
                return left
            elif nums[mid] > target:
                left = mid + 1
            else:
                right = mid - 1

        return -1
```
🔴 **Why it failed?**
- I didn't correctly identify whether the left or right half was sorted.
- The `left = mid + 1` and `right = mid - 1` conditions were **incorrectly placed**, leading to failure in rotated cases.

---

## **✅ Optimized Solution (Correct)**
### **🚀 Correct Approach**
- **Identify the sorted half** → Since the array is rotated, one half will always be sorted.
- **Check if the target lies in that sorted half**:
  - If **left half is sorted** (`nums[left] <= nums[mid]`), check if `target` lies between `nums[left]` and `nums[mid]`.
  - Else, search in the right half.
- **Binary Search continues until left pointer crosses right**.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid  # Found target
            
            # Check if left half is sorted
            if nums[left] <= nums[mid]:  
                # Target is in the left sorted half
                if nums[left] <= target < nums[mid]:  
                    right = mid - 1  
                else:
                    left = mid + 1  
            else:  
                # Right half is sorted
                if nums[mid] < target <= nums[right]:  
                    left = mid + 1  
                else:
                    right = mid - 1  

        return -1
```

---

## **📝 Key Takeaways**
### **What I Learned**
✔ **Binary Search in Rotated Arrays requires checking sorted halves first**  
✔ **Always check if `nums[mid] == target` first to avoid unnecessary steps**  
✔ **Identifying the sorted half helps determine where the target is present**  

---

## **⏳ Time & Space Complexity**
| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| **First Attempt (Incorrect)** | ❌ `O(log n)` (but incorrect logic) | `O(1)` |
| **Final Optimized Solution** | ✅ `O(log n)` (binary search) | `O(1)` |

---

## **🔥 Final Thoughts**
This problem was a great **binary search variation**, and after struggling with the first approach, I fully grasped how to efficiently find the minimum and apply binary search **even in rotated arrays**. **Feeling more confident with Binary Search now! 🚀**

---