## 概要

https://leetcode.com/problems/number-of-islands/description/

> Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.
> 
> 
> An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
> 

## Intuition

下、上、右、左、という概念が出てきた。一度のTraverseでいけたところまでを１つの島として、連続した箇所として記録する。

Traverse済みの箇所は再探索しないようにしながら、全部の要素を探索する。

幅探索が向いていそう。

## 計算量

O(N*M)

## Code

グラフアルゴリズムは、BFSとDFSの基礎を身につけて仕舞えば応用が効く。

が、Iterateする部分が多いから直感的ではない。

2Dの地図の場合、row(列)をx, col(行)をyにしてTraverseする。

ベースケースはstackがemptyな時か範囲外に出た時。

周囲の連続する陸地をひたすら調べて、調べた場所は探索済みフラグを立てる。

```cpp
#include <iostream>
#include <vector>

using namespace std;

class Solution {
public:
    void dfs(vector<vector<char>>& grid, int i, int j) {
        int m = grid.size();
        int n = grid[0].size();
        
        // Check if we are out of bounds or at a water cell ('0')
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == '0') {
            return;
        }
        
        // Mark the current cell as visited by setting it to '0'
        grid[i][j] = '0';
        
        // Visit all adjacent cells (up, down, left, right)
        dfs(grid, i + 1, j);  // Down
        dfs(grid, i - 1, j);  // Up
        dfs(grid, i, j + 1);  // Right
        dfs(grid, i, j - 1);  // Left
    }

    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) return 0;
        
        int numIslands = 0;
        int m = grid.size();
        int n = grid[0].size();
        
        // Traverse each cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {  // Start of a new island
                    numIslands++;  // Increment the island count
                    dfs(grid, i, j);  // Perform DFS to mark all connected land
                }
            }
        }
        
        return numIslands;  // Return the total number of islands found
    }
};

int main() {
    Solution solution;
    vector<vector<char>> grid1 = {
        {'1', '1', '1', '1', '0'},
        {'1', '1', '0', '1', '0'},
        {'1', '1', '0', '0', '0'},
        {'0', '0', '0', '0', '0'}
    };
    vector<vector<char>> grid2 = {
        {'1', '1', '0', '0', '0'},
        {'1', '1', '0', '0', '0'},
        {'0', '0', '1', '0', '0'},
        {'0', '0', '0', '1', '1'}
    };

    cout << "Number of Islands in grid1: " << solution.numIslands(grid1) << endl;  // Output: 1
    cout << "Number of Islands in grid2: " << solution.numIslands(grid2) << endl;  // Output: 3

    return 0;
}

```

```cpp
class Solution {
public:
    void dfsIterative(vector<vector<char>>& grid, int i, int j) {
        int m = grid.size();
        int n = grid[0].size();

        // Use a stack to simulate the recursive DFS
        stack<pair<int, int>> stk;
        stk.push({i, j});

        // Mark the starting cell as visited by setting it to '0'
        grid[i][j] = '0';

        // Directions for moving up, down, left, right
        vector<pair<int, int>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        while (!stk.empty()) {
            auto [x, y] = stk.top();
            stk.pop();

            // Visit all 4 possible directions
            for (auto& dir : directions) {
                int newX = x + dir.first;
                int newY = y + dir.second;

                // Check if the new position is within bounds and is land ('1')
                if (newX >= 0 && newX < m && newY >= 0 && newY < n && grid[newX][newY] == '1') {
                    // Mark as visited
                    grid[newX][newY] = '0';
                    stk.push({newX, newY});
                }
            }
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty()) return 0;

        int numIslands = 0;
        int m = grid.size();
        int n = grid[0].size();

        // Traverse each cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {  // Start of a new island
                    numIslands++;  // Increment the island count
                    dfsIterative(grid, i, j);  // Perform iterative DFS to mark all connected land
                }
            }
        }

        return numIslands;  // Return the total number of islands found
    }
};

int main() {
    Solution solution;
    vector<vector<char>> grid1 = {
        {'1', '1', '1', '1', '0'},
        {'1', '1', '0', '1', '0'},
        {'1', '1', '0', '0', '0'},
        {'0', '0', '0', '0', '0'}
    };
    vector<vector<char>> grid2 = {
        {'1', '1', '0', '0', '0'},
        {'1', '1', '0', '0', '0'},
        {'0', '0', '1', '0', '0'},
        {'0', '0', '0', '1', '1'}
    };

    cout << "Number of Islands in grid1: " << solution.numIslands(grid1) << endl;  // Output: 1
    cout << "Number of Islands in grid2: " << solution.numIslands(grid2) << endl;  // Output: 3

    return 0;
}

```
