> Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.
> 
> 
> **Note** that you must do this in-place without making a copy of the array.
> 

https://leetcode.com/problems/move-zeroes/description/

## Intuition

- ０じゃない要素の数を数える作業
- 0で残りを埋める作業

## Code

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int nonZeroIndex = 0;
        // increment nonzeroindex if it is not 0
        for(int i = 0; i < nums.size(); i++) {
            if(nums[i] != 0) {
                nums[nonZeroIndex]  = nums[i];
                nonZeroIndex++;
            }
        }
        // nonZeroIndex points to total sum of non zero elements.
        // so, nums.size - nonZeroIndex will be total zero elements in array.
        for(int i = nonZeroIndex; i < nums.size(); i++) {
            nums[i] = 0;
        }
    }
};
```
