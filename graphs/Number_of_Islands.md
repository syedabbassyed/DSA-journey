### **Number of Islands**  
**Leetcode #200 | Difficulty: Medium**  
**Category:** Graph / DFS  
**Pattern:** Depth-First Search (DFS)

---

### **Problem Statement**

Given an `m x n` 2D grid map of `'1'`s (land) and `'0'`s (water), return the number of islands.  
An island is surrounded by water and is formed by connecting adjacent lands **horizontally or vertically**.

---

### **Example 1**
**Input:**
```
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
```
**Output:** `1`

---

### **Example 2**
**Input:**
```
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```
**Output:** `3`

---

### **My Final Solution (DFS)**

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        rows = len(grid)
        cols = len(grid[0])
        island_count = 0

        def dfs(r, c):
            if r < 0 or c < 0 or r >= rows or c >= cols or grid[r][c] != '1':
                return
            grid[r][c] = '0'  # Mark visited
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1':
                    island_count += 1
                    dfs(r, c)

        return island_count
```

---

### **What I Learned**
- How to apply **DFS** to **2D grid problems**.
- Treat each `'1'` as a new land and explore all connected land.
- **Recursive DFS** helps eliminate all adjacent `'1'`s once visited.
- Grid traversal problems often require careful boundary checks.
