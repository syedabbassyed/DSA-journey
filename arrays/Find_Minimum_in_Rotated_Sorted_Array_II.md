### 🧠 Problem: Find Minimum in Rotated Sorted Array II  
**Leetcode #154 | Difficulty: Hard | Pattern: Binary Search with Duplicates**

---

### ✅ Problem Statement:
Given a sorted array that may contain **duplicates** and has been rotated at an unknown pivot, find the **minimum** element.

You must write an algorithm that runs in **O(log n)** time in the average case.

---

### 🧪 First Thoughts:

I had previously solved **LeetCode 153 (no duplicates)**, so I used the same approach but was cautious of **duplicates**, which make the binary search a bit ambiguous.

I knew that when `nums[mid] == nums[right]`, we can't determine which side is sorted, so we just **shrink** the search space.

---

### ✅ Final Code (Solved in 1 min after clue):

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if nums[mid] == nums[right]:
                right -= 1  # shrink the range
            elif nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid

        return nums[left]
```

---

### 🧠 Key Concepts:
- **Binary Search** logic carried over from `153`.
- Added check for **duplicates**.
- Shrinking `right` when `nums[mid] == nums[right]` is the key insight.
- Time complexity:  
  - Best/Average: **O(log n)**  
  - Worst (all duplicates): **O(n)**

---

### 🔥 Pattern Learned:
- **Binary Search with Duplicates**:  
  When `nums[mid] == nums[right]`, the only safe move is to shrink `right -= 1`. You can’t determine the sorted half like you normally do.

---