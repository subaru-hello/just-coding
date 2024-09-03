## 概要

https://leetcode.com/problems/two-sum/description/

> Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.
> 
> 
> You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.
> 
> You can return the answer in any order.
> 

numsをひとつづつ取り出して、足し合わせた数がtargetになったら二つの値を返却する。

## Intuition

最初の値をvectorに入れて、target - 最初の値が出てきたら、vectorに入れる。

最後にvectorを返却する。O(N)になりそう

## Complexity

O(N*2) brute force searching

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
       // brute force search
       int size = nums.size();
       for (int i = 0; i < size - 1; i ++) {
            for(int j = i + 1; j < size; j ++) {
                if(nums[i] + nums[j] == target){
                    return {i,j};
                }
            } 
       }
        return {};
    }
};
```

Two Pass hash Table

```cpp

```
