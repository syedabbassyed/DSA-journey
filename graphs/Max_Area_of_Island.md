### **Max Area of Island**  
📌 **Category**: Graphs / DFS on Grid  
💻 **Leetcode**: [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/)  
🧠 **Difficulty**: Medium  
🧪 **Pattern**: DFS, Matrix Traversal, Connected Components  

---

### ✅ **Problem Statement**  
Given a 2D grid of 0s (water) and 1s (land), return the maximum area of an island in the grid.  
An island is formed by connecting adjacent 1s (horizontally or vertically).

---

### 🚧 **First Attempt (Failed - Python Recursion Bug)**

```python
def dfs_area(r, c, area):
    if r < 0 or c < 0 or r >= rows or c >= cols or grid[r][c] != 1:
        return area
    
    grid[r][c] = 0
    area += 1

    dfs_area(r + 1, c, area)
    dfs_area(r - 1, c, area)
    dfs_area(r, c + 1, area)
    dfs_area(r, c - 1, area)
```

❌ **Mistake**: Expected `area` to mutate across recursive calls, but Python integers are immutable.  
✅ **Realization**: Return the area from each recursive call and sum them instead.

---

### ✅ **Final Working Solution**

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        rows = len(grid)
        cols = len(grid[0])
        max_area = 0

        def dfs_area(r, c):
            if r < 0 or c < 0 or r >= rows or c >= cols or grid[r][c] != 1:
                return 0
            
            grid[r][c] = 0
            area = 1

            area += dfs_area(r + 1, c)
            area += dfs_area(r - 1, c)
            area += dfs_area(r, c + 1)
            area += dfs_area(r, c - 1)

            return area

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    max_area = max(max_area, dfs_area(r, c))
        
        return max_area
```

---

### 🔑 **Key Learnings**

- ❗ **Mutable vs Immutable in Python**: You cannot rely on passing an `int` to mutate inside recursion. Always return accumulated values explicitly.
- 🧠 **DFS Traversal**: This is a classic **connected component** problem — apply DFS to flood all `1`s connected to the current node.
- 📈 **Pattern Connection**: Reused logic from [Number of Islands](./graphs/Number_of_Islands.md), but modified to track **area** instead of **count**.
- 💬 **Interview-Ready Thinking**: You showed self-correction after feedback, rewrote the logic cleanly, and handled all base cases properly.

---