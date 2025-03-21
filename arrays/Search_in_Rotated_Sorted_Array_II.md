### 🔍 Leetcode 81 - Search in Rotated Sorted Array II  
**Category:** Arrays  
**Difficulty:** Medium  
**Pattern:** Binary Search (with Duplicates Handling)  
**Tags:** Binary Search, Rotated Sorted Array, Edge Case Handling

---

## 🧠 Problem Summary

You are given an array of integers `nums` that may contain duplicates, sorted in **ascending order**, but rotated at an unknown pivot.  
Determine if a given `target` exists in `nums`.

---

## 🧪 Edge Cases Considered
- Duplicates (e.g., `[1, 0, 1, 1, 1]`)
- Left = mid = right scenario (ambiguous pivot)
- Fully sorted array (no rotation)
- All elements same except target
- Empty array

---

## ✅ First Attempt:  
Tried the base logic with pivot detection but did not initially handle `left == mid == right`, which caused failure on duplicate-heavy arrays.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == nums[right]:
                right -= 1
            else:
                if nums[mid] == target:
                    return True
                else:
                    if nums[left] < nums[mid]:
                        if nums[left] <= target < nums[mid]:
                            right = mid - 1
                        else:
                            left = mid + 1
                    else:
                        if nums[mid] < target <= nums[right]:
                            left = mid + 1
                        else:
                            right = mid - 1

        return False
```

---

## ⚙️ Second Attempt: Minor Optimization  
Moved equality check for `nums[mid] == target` to the top and simplified conditions.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return True
            else:
                if nums[mid] == nums[right]:
                    right -= 1
                else:
                    if nums[left] < nums[mid]:
                        if nums[left] <= target < nums[mid]:
                            right = mid - 1
                        else:
                            left = mid + 1
                    else:
                        if nums[mid] < target <= nums[right]:
                            left = mid + 1
                        else:
                            right = mid - 1

        return False
```

---

## 🧠 Final Optimized Version (Edge Case Handled)  
Added the crucial check for ambiguous condition: `nums[left] == nums[mid] == nums[right]`. This can appear when array contains many duplicates and breaks binary search guarantees.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return True
            if nums[left] == nums[mid] == nums[right]:
                left += 1
                right -= 1
            elif nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return False
```

---

## ✅ What I Learned
- Binary search in rotated arrays becomes tricky with **duplicates**.
- The ambiguous condition `left == mid == right` must be handled carefully, else it **breaks the search direction**.
- It’s essential to **build logic for unbalanced halves** when duplicates interfere with sorted segments.
- **Binary search is not always strictly about halving — it's about intelligently eliminating halves.**

---