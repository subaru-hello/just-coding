## Problem

https://leetcode.com/problems/minimum-size-subarray-sum/

> 
> 
> 
> Given an array of positive integers nums and a positive integer target, return *the **minimal length** of a subarray whose sum is greater than or equal to target*. 
> 
> If there is no such subarray, return 0 instead
> 

## Intuition

スライディングウィンドウアルゴリズムを使って、範囲内の最小値を追う。

## Code

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size();
        int min_len = INT_MAX;
        int left = 0, sum = 0;

        for(int right = 0; right < n; right++) {
            sum += nums[right];
            while(sum >= target) {
                min_len = min(min_len, right - left + 1);
                sum -= nums[left];
                left++;
            }
        }
        
        return (min_len == INT_MAX) ? 0 : min_len;
    }
};l
```
