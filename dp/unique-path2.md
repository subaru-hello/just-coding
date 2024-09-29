## 概要

https://leetcode.com/problems/unique-paths-ii/description/

> You are given an `m x n` integer array `grid`. There is a robot initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.
> 
> 
> An obstacle and space are marked as `1` or `0` respectively in `grid`. A path that the robot takes cannot include **any** square that is an obstacle.
> 
> Return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.
> 
> The testcases are generated so that the answer will be less than or equal to `2 * 109`.
> 

## Intuition

あるマスは、左か上からしかアクセスできないため、一番左の列と一番上の行のマス達はひとつのルートでしかアクセスできないことが決まる。

そのため、grid[m][0]とgrid[0][n]は全て1になる。

obstacleかマスが続いている限り、そのマスへアクセスするパターンは上と左のマスのアクセス方法のどちらか大きい方＋１になる。

もしobstacleの場合、その道は0になる。

## Code

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        if(m==0) return 0;
        int n = obstacleGrid[0].size();
        vector<vector<int>>dp(m, vector<int>(n,0));
        dp[0][0] = (obstacleGrid[0][0]==0) ? 1 : 0;
        for(int i = 1; i < m; i++) {
            // no obstacle. previous space has a path.
            if(obstacleGrid[i][0] == 0 && dp[i-1][0] == 1) {
                dp[i][0] = 1;
            } else {
                dp[i][0] = 0;
            }
        }

        for(int j = 1; j < n; j++) {
            if(obstacleGrid[0][j] == 0 && dp[0][j-1] == 1) {
                dp[0][j] = 1;
            } else {
                dp[0][j] = 0;
            }
        }

        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                if(obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                } else {
                    dp[i][j] = 0;
                }
            }
        }

        return dp[m-1][n-1];
    }
};
```
