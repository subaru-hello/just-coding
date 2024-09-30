## 概要

https://leetcode.com/problems/house-robber/description/

> 
> 
> 
> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night.**
> 
> Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.
> 

## Intuition

Dynamic Programingを使う。

Dynamic Programingは、大きな問題を個別の問題にして小さくなった問題をボトムアップに解いていく解法のことを指す。

今回の問題の条件を整理する。

- 隣接した家のお金は取れない
- 取れる分だけお金を取る

ひとつ目の条件は、dp[i-2]+nums[i]と現状取れうる最大のお金を比べて、大きい方を選べば実現できそう。

二つ目の条件は、dp[i-1]に入っている値を返せば実現できる。

## Code

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        // Base case if nums is empty
        if(nums.empty()) return 0;
        // if nums is only 1 element, return the first money;
        int n = nums.size();
        if(n == 1) return nums[0];
        // initialize dp vector
        vector<int> dp(n,0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        // divide and conquer the dp
        for(int i = 2; i < n; i ++) {
            dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
        }
        return dp[n-1];
    }
};
```

dpに保存されるのは直前二つだけなので、Space Complexityを効率化するために、vectorではなくintの箱を二つだけにすることもできる。

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.empty()) return 0;
        int n = nums.size();
        int prev1 = 0; // dp[n-1];
        int prev2 = 0; // dp[n-2];
        for(int& num: nums) {
            int tmp = prev1;
            prev1 = max(prev1, prev2 + num);
            prev2 = tmp;
        }
        return prev1;
    }
};
```
