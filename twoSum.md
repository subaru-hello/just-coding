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

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> numMap;
        int n = nums.size();
				// 
        for(int i = 0; i < n; i++) {
            int complement = target - nums[i];
            if(numMap.count(complement)) {
                return {numMap[complement], i};
            }
            numMap[nums[i]] = i;
        }
        return {};
    }
};
```

## Similar Algorithms

1. [Two Sum](https://leetcode.com/problems/two-sum/solutions/3619262/3-method-s-c-java-python-beginner-friendly/)
2. [Roman to Integer](https://leetcode.com/problems/roman-to-integer/solutions/3651672/best-method-c-java-python-beginner-friendly/)
3. [Palindrome Number](https://leetcode.com/problems/palindrome-number/solutions/3651712/2-method-s-c-java-python-beginner-friendly/)
4. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/solutions/3666304/beats-100-c-java-python-beginner-friendly/)
5. [Remove Element](https://leetcode.com/problems/remove-element/solutions/3670940/best-100-c-java-python-beginner-friendly/)
6. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/solutions/3672475/4-method-s-c-java-python-beginner-friendly/)
7. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/solutions/3675747/beats-100-c-java-python-beginner-friendly/)
8. [Majority Element](https://leetcode.com/problems/majority-element/solutions/3676530/3-methods-beats-100-c-java-python-beginner-friendly/)
9. [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/solutions/3676877/best-method-100-c-java-python-beginner-friendly/)
