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

```
