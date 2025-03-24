### **Number of Enclaves**  
**Leetcode #1020 | Difficulty: Medium**  
**Category:** Graph / DFS  
**Pattern:** Depth-First Search (DFS)  

---

### **Problem Statement**  
Given a 2D binary matrix `grid` where `1` represents land and `0` represents water, return the number of land cells from which we **cannot walk off the boundary** of the grid.

A land cell is considered an **enclave** if it cannot reach the grid boundary by moving only in 4 directions (up, down, left, right).

---

### **Example 1**  
**Input:**  
```
grid = [
  [0,0,0,0],
  [1,0,1,0],
  [0,1,1,0],
  [0,0,0,0]
]
```
**Output:** `3`

---

### **Example 2**  
**Input:**  
```
grid = [
  [0,1,1,0],
  [0,0,1,0],
  [0,0,1,0],
  [0,0,0,0]
]
```
**Output:** `0`

---

### **My Final Solution (DFS)**  
```python
class Solution:
    def numEnclaves(self, grid: List[List[int]]) -> int:
        rows = len(grid)
        cols = len(grid[0])

        def dfs(r, c):
            if r < 0 or c < 0 or r >= rows or c >= cols or grid[r][c] != 1:
                return
            grid[r][c] = 0
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for c in range(cols):
            if grid[0][c] == 1:
                dfs(0, c)
            if grid[rows - 1][c] == 1:
                dfs(rows - 1, c)

        for r in range(rows):
            if grid[r][0] == 1:
                dfs(r, 0)
            if grid[r][cols - 1] == 1:
                dfs(r, cols - 1)

        moves = 0
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    moves += 1

        return moves
```

---

### **What I Learned**
- 🔁 **Flood fill technique** on the **boundary land cells** can eliminate invalid enclaves.
- ✅ Any land cell (`1`) that is connected to the edge should be marked and ignored from the count.
- 💡 Smart boundary-only DFS saves time vs scanning entire grid multiple times.
- 🧠 Remember: Python’s recursion has a depth limit (~1000), so for large grids, iterative DFS or BFS may be needed in interviews.