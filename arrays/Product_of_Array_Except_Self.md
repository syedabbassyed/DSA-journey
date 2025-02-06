### **Product of Array Except Self (LeetCode #238 - Medium)**  

#### **Problem Statement**  
Given an integer array `nums`, return an array `result` such that `result[i]` is the product of all elements in `nums` **except** `nums[i]`.  

**Constraint:**  
- You **must not** use division.
- The solution must run in **O(n) time**.

---

## **🛠️ First Approach: Brute Force (Nested Loops)**
### **Idea:**  
- For each element `nums[i]`, iterate through the entire array and multiply every element **except** `nums[i]`.
- This leads to an **O(n²) time complexity**, which is too slow for large inputs.

### **Implementation:**  
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        finalProductArray = []
        for i in range(len(nums)):
            product = 1
            for j in range(len(nums)):
                if i != j:
                    product *= nums[j]
            finalProductArray.append(product)
        return finalProductArray
```

### **Why This Fails:**  
🚫 **Time Complexity:** **O(n²)** → Too slow for large inputs.  
🚫 **Redundant Computations:** We keep recalculating products multiple times instead of reusing previously computed values.  

---

## **🛠️ Second Approach: Using Prefix & Suffix Products**
### **Idea:**  
- Instead of recalculating products every time, store **prefix** (product of elements **before** `i`) and **suffix** (product of elements **after** `i`).  
- Multiply the **prefix product** and **suffix product** to get the final result.  

### **Implementation:**  
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        prefix_product = [1] * n
        suffix_product = [1] * n
        result = [1] * n

        # Compute prefix products
        for i in range(1, n):
            prefix_product[i] = prefix_product[i - 1] * nums[i - 1]
        
        # Compute suffix products
        for i in range(n - 2, -1, -1):
            suffix_product[i] = suffix_product[i + 1] * nums[i + 1]
        
        # Multiply prefix and suffix products
        for i in range(n):
            result[i] = prefix_product[i] * suffix_product[i]
        
        return result
```

### **Complexity Analysis:**  
✅ **Time Complexity:** **O(n)** → We traverse the array **3 times**, but it’s still linear.  
✅ **Space Complexity:** **O(n)** → Uses extra arrays for prefix & suffix products.  

### **Why This is an Improvement:**  
- We avoid redundant calculations and reduce the nested loop to **three separate passes**.
- However, **it still uses extra space** (`O(n)`) for prefix & suffix arrays.

---

## **✅ Optimized Approach: In-Place Calculation**
### **Idea:**  
- Instead of using two separate arrays (`prefix_product` and `suffix_product`), we compute everything in **one output array (`result`)**.
- First, store **prefix product** in `result`.  
- Then, traverse **backward** to multiply it with the **suffix product** dynamically.  

### **Optimized Implementation:**  
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        result = [1] * n
        
        # Compute prefix products directly in result
        prefix_product = 1
        for i in range(n):
            result[i] = prefix_product
            prefix_product *= nums[i]

        # Compute suffix products and multiply with prefix in result array
        suffix_product = 1
        for i in range(n - 1, -1, -1):
            result[i] *= suffix_product
            suffix_product *= nums[i]
        
        return result
```

### **Complexity Analysis:**  
✅ **Time Complexity:** **O(n)** → We traverse the array **twice**, so it's still linear.  
✅ **Space Complexity:** **O(1)** → **No extra space** used except for the output array (`result`).  

---

## **Example Walkthrough**
### **Input:**
```python
nums = [1, 2, 3, 4]
```
### **Steps:**
1️⃣ **Compute Prefix Product:**  
```
result = [1, 1, 2, 6]
```
(Stores the product of elements before `i`)

2️⃣ **Compute Suffix Product & Multiply:**  
```
result = [24, 12, 8, 6]
```
(Final result after incorporating suffix product)

### **Output:**
```python
[24, 12, 8, 6]
```

---

## **🔑 Key Takeaways**
| Approach        | Time Complexity | Space Complexity | Key Idea |
|----------------|---------------|----------------|----------|
| **Brute Force** | **O(n²)** | **O(1)** | Multiply all elements except `nums[i]` using nested loops |
| **Prefix & Suffix Products** | **O(n)** | **O(n)** | Store prefix & suffix in two arrays and multiply |
| **Optimized (In-Place)** | **O(n)** | **O(1)** | Compute prefix in `result`, then update it with suffix |

🔥 **Final Optimized Solution:** Uses **only O(1) extra space** while keeping **O(n) time complexity**.  

---

## **🌟 Lessons Learned**
- **Using extra space can simplify problems at first**, but always look for ways to **optimize space**.  
- **Prefix & Suffix products** are powerful techniques to eliminate unnecessary calculations.  
- **In-place calculations** help achieve **O(1) space** optimization.  