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

time O(N) at most.

**Space Complexity:** O(n) 

In the worst case, all elements in nums are unique, so numMap will store n elements.

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        // init a numMap to hold indexes for values in nums;
        unordered_map<int, int> numMap;
        int n = nums.size();
        // build a numMap
        for(int i = 0; i < n; i++) {
            numMap[nums[i]] = i;
        }

        // if target - numMap[nums[i]] contain a numMap and not a same return i and numMap index
        for(int i = 0; i < n; i++) {
            int complement = target - nums[i];
            if(numMap.count(complement) && numMap[complement] != i) {
                return {i, numMap[complement]};
            }
        }
        return {};
    }
};
```

**One-pass Hash Table**


