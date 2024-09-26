## 概要

https://leetcode.com/problems/unique-paths/description/

> 
> 
> 
> There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.
> 
> Given the two integers `m` and `n`, return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.
> 
> The test cases are generated so that the answer will be less than or equal to `2 * 109`.
> 

## Intuition

ゴールに到達するまでの歩数を累積わでマス目に登録していく。

マス目は、上と左のマス目に書いてある値を足し合わせた数が入る。

## Code

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
       // to hold a 2D vector
       vector<vector<int>> dp(m, vector<int>(n,1));
       for(int i = 1; i < m; i++) {
         for (int j = 1; j < n; j++) {
            // update current traversaling space with value(top + left)
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
         }
       }
       return dp[m-1][n-1];
    }
};
```
