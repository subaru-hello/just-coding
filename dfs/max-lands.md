## 概要

> 
> 
> 
> You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.
> 
> The **area** of an island is the number of cells with a value `1` in the island.
> 
> Return *the maximum **area** of an island in* `grid`. If there is no island, return `0`.
> 

## Intuition

前回と違ってInt型だ。

gridをTraverseする。

1を見つけ次第、1の数をDFSかBFSで数える。

探索済みにしながら、1の数をincrement

1の数が4以上ならmaximumLandを1incrementする

Traverseは例によって上下左右行う。gridの探索は２次元なので、grid[i][j]になる。

## Complexity

## Code

```cpp
class Solution {
public:
    int dfs(vector<vector<int>>& grid, int i, int j) {
        int m = grid.size();
        int n = grid[0].size();
        
        // Check if out of bounds or at a water cell ('0')
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0) {
            return 0;
        }

        // Mark the current cell as visited by setting it to '0'
        grid[i][j] = 0;

        // Start with an area of 1 (current cell)
        int area = 1;

        // Visit all adjacent cells (up, down, left, right)
        area += dfs(grid, i + 1, j);  // Down
        area += dfs(grid, i - 1, j);  // Up
        area += dfs(grid, i, j + 1);  // Right
        area += dfs(grid, i, j - 1);  // Left

        return area;  // Return the area of the current island
    }

    int maxAreaOfIsland(vector<vector<int>>& grid) {
        if (grid.empty()) return 0;
        
        int maxArea = 0;
        int m = grid.size();
        int n = grid[0].size();
        
        // Traverse each cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {  // Start of a new island
                    int currentArea = dfs(grid, i, j);  // Calculate the area of this island
                    maxArea = max(maxArea, currentArea);  // Update the maximum area found
                }
            }
        }
        
        return maxArea;  // Return the maximum area of an island
    }
};
```
