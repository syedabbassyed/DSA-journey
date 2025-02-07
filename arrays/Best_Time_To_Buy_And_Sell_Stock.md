### **Best Time to Buy and Sell Stock (LeetCode #121 - Easy)**  

#### **Problem Statement**  
Given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day, find the **maximum profit** you can achieve from a single buy and sell.  
- You **must buy before selling**.  
- If no profit can be made, return `0`.  

---

## **🛠️ First Approach: Brute Force (Tracking Buy & Sell Indices)**
### **Idea:**  
- Track the **best buy price** and its index, then track the **best sell price** after buying.  
- If a lower price appears, update `buy` and reset `sell`.  
- This solution works but has unnecessary complexity.

### **Implementation:**  
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        buy = [float('inf'), 0]
        sell = [0, float('inf')]
        n = len(prices)
        profit = 0
        for i in range(n):
            if prices[i] < buy[0] and (sell[1] > i or i != n -1):
                buy = [prices[i], i]
                sell = [0, float('inf')]
            if prices[i] > sell[0] and i > buy[1] and prices[i] > buy[0]:
                sell = [prices[i], i]
                if (sell[0] - buy[0]) > profit:
                    profit = sell[0] - buy[0]
        
        return profit
```

### **Complexity Analysis:**  
🚫 **Time Complexity:** **O(n)** → Good, but extra checks make it more complex than necessary.  
🚫 **Space Complexity:** **O(1)** → No extra data structures, but tracking indices is unnecessary.  

### **Why This Can Be Improved?**  
- The conditions in the loop are **more complex than needed**.  
- We don’t really need to **explicitly track both buy & sell indices**—just keeping track of `min_price` is enough.

---

## **🛠️ Second Approach: Sliding Window**
### **Idea:**  
- Use **two pointers (`left` and `right`)** as a **moving window**.  
- `left` points to the **cheapest buy price**, while `right` scans ahead to find a **higher sell price**.  
- If `right` finds a **better selling price**, update `maxProfit`.  
- If `right` finds a **cheaper buying price**, move `left` forward.

### **Implementation:**  
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        left, right = 0, 1
        maxProfit = 0

        while right < len(prices):
            if prices[left] < prices[right]:
                profit = prices[right] - prices[left]
                maxProfit = max(maxProfit, profit)
            else:
                left = right
            right += 1
        
        return maxProfit
```

### **Complexity Analysis:**  
✅ **Time Complexity:** **O(n)** → Single pass through the array.  
✅ **Space Complexity:** **O(1)** → Uses only a few variables.  

### **Why This Works Well?**  
- It **effectively simulates** buying and selling by adjusting the `left` pointer whenever we find a lower price.  
- However, we **don’t actually need both left & right pointers**—we can simplify it using a **greedy approach**.

---

## **🛠️ Third Approach: One-Pass Greedy Algorithm (Optimal Solution)**
### **Idea:**  
- Instead of using two pointers, just track the **minimum price (`min_price`)** while iterating.  
- **At each step, check:** If we **bought at `min_price` and sold today, what's the profit?**  
- Update `max_profit` accordingly.

### **Implementation:**  
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = float('inf')  # Initialize to a very high value
        max_profit = 0  # Track the best possible profit

        for price in prices:
            if price < min_price:  
                min_price = price  # Update the lowest buying price
            else:
                max_profit = max(max_profit, price - min_price)  # Check potential profit

        return max_profit
```

### **Complexity Analysis:**  
✅ **Time Complexity:** **O(n)** → Single pass through the array.  
✅ **Space Complexity:** **O(1)** → Uses only two extra variables.  

### **Why This is the Best Approach?**  
✔ **Simplest Implementation** → No need to track indices or use extra conditions.  
✔ **Faster Execution** → Just keeps track of `min_price` and updates `max_profit`.  
✔ **Greedy Approach** → Makes a decision at every step: "Is this the best time to buy or sell?"  

---

## **Example Walkthrough**
### **Input:**  
```python
prices = [7, 1, 5, 3, 6, 4]
```

### **Step-by-Step Execution:**  

| Day | Price | `min_price` (Best Buy) | `max_profit` (Best Profit) |
|------|--------|----------------|------------------|
| 1 | 7 | **7** | 0 |
| 2 | 1 | **1** (New min) | 0 |
| 3 | 5 | 1 | **4** (`5 - 1 = 4`) |
| 4 | 3 | 1 | **4** |
| 5 | 6 | 1 | **5** (`6 - 1 = 5`) |
| 6 | 4 | 1 | **5** |

### **Output:**  
```python
5
```
✔ Buy at **1**, Sell at **6** → **Profit = 5** ✅  

---

## **🔑 Key Takeaways**
| Approach | Time Complexity | Space Complexity | Key Idea |
|------------|----------------|----------------|----------------|
| **Brute Force** | **O(n)** | **O(1)** | Tracks buy & sell explicitly but adds unnecessary complexity |
| **Sliding Window** | **O(n)** | **O(1)** | Uses two pointers to track buy & sell dynamically |
| **Greedy (Optimal)** | **O(n)** | **O(1)** | Tracks only `min_price` while iterating, making it the simplest |

🔥 **Final Verdict:** The **greedy approach** is the most **elegant and efficient solution**. 🚀  

---

## **🌟 Lessons Learned**
- **Greedy algorithms** work well when **finding an optimal min/max over a sequence**.  
- **Sliding Window isn’t always needed**—sometimes a **single-pass greedy approach** is more intuitive.  
- **Tracking only the necessary variables** keeps the solution clean and efficient.  