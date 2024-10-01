## 概要

https://leetcode.com/problems/house-robber-ii/description/

> 
> 
> 
> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
> 
> Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.
> 

## Intuition

条件を整理する。

- 隣接する家からお金を取ることはできない
- 家はサークル状に並んでいる。最初と最後は隣り合わせになる。

確保すべきスペースは、前回と同様２つだけになりそう(dp[i-1],  dp[i-2])

最初と最後が隣接するため、dpを実行するときは2回に分ける必要がある。

- 最初の家から最後から１つ手前までの中から盗む家を選ぶ
- 最初の次から最後までの中から盗む家を選ぶ

## Code

Space ComplexityがO(1)になる書き方

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        // we can not rob a first and last house at a same iteration
        if(nums.empty()) return 0;
        int n = nums.size(); if(n==1) return nums[0];

        // rob first house, excluding a last house.
        int robStart = helper(nums,0,n-1);

        // rob first last, excluding a first house.
        int robEnd = helper(nums,1,n);

        // select a plan which I could obtain a max money.
        int maxMoney = max(robStart, robEnd);
        return maxMoney;
    }

    // house robber minimal helper
    int helper(vector<int>& nums, int start, int end) {
        int prev2 = 0; // dp[i-2]
        int prev1 = 0; // dp[i-1]
        for(int i = start; i < end; i++) {
            int tmp = prev1;
            prev1 = max(prev1, prev2 + nums[i]);
            prev2 = tmp;
        }
        return prev1;
    }
};
```
