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

O(n*m)

## Code

bfsのコードを作って、現在地点とbfsで探索した陸地を比べて大きい方を取得し、個数を返却する。

bfsは、stackと探索済みvectorを使って実装。

stackに現在地点を入れて探索済みにする。

上下左右を探索し、1(陸地)だったらstackへ入れて陸地intを1incrementし、stackに探索中の陸をひとつpushする。

これを、stackが空になる(隣接する陸がなくなる)まで繰り返す。

```cpp
class Solution {
    public:
    int bfs(vector<vector<int>>&grid, int i, int j) {
        int row = grid.size();
        int col = grid[0].size();
        stack<pair<int, int>>stk;
        stk.push({i,j});

        vector<pair<int,int>>directions ={{1,0},{-1,0},{0,1},{0,-1}};
        // mark starting point as traversed;
        grid[i][j] = 0;
        // contiguous land area. given that this is a first land set1;
        int land = 1;
        while(!stk.empty()){
          auto [x,y] = stk.top();
          stk.pop();
          for(auto& dir: directions){
            int newX = x + dir.first;
            int newY = y + dir.second;
            // 範囲内かつ陸の場合
            if(newX >=0 && newX < row && newY >=0 && newY < col && grid[newX][newY]==1){
                land++;
                grid[newX][newY]=0;
                stk.push({newX,newY});
            }
          }
        }
        return land;
    };

    int maxAreaOfIsland(vector<vector<int>>&grid) {
        int row = grid.size();
        int col = grid[0].size();
        int maxLand = 0;
        for(int i = 0; i< row; ++i){
            for(int j = 0; j < col; ++j){
                if(grid[i][j] == 1){
                    // 隣接する陸の数
                    int currentArea = bfs(grid,i,j);
                    // maxの陸続きを更新する
                    maxLand = max(maxLand,currentArea);
                }
            }
        }
        return maxLand;
    }
};};
```
