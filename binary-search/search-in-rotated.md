## 概要

https://leetcode.com/problems/search-in-rotated-sorted-array/description/

> There is an integer array `nums` sorted in ascending order (with **distinct** values).
> 
> 
> Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.
> 
> Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of* `target` *if it is in* `nums`*, or* `-1` *if it is not in* `nums`.
> 
> You must write an algorithm with `O(log n)` runtime complexity.
> 

## Intuition

まず、ソート済みでdistinctな値であることに注目。

- ローテートされた結果、midがminより大きいか
    - 大きい場合、左はmidより小さい、みぎはmidより大きいと言うことがわかる。
    - なので、targetが左にある場合、最大値(右)をmid -1にして範囲を左側のみに絞る
    - そうでない場合、targetは右側にあるため、最小値(左)をmid + 1にして範囲を右側に絞る

## Complexity

time: O(log n)

## Code

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
       // base case. if nums empty there is no target.
        if(nums.empty()) return -1;
        int min = 0, max = nums.size() -1;
       // while min is less than max
        while(min <= max) {
            int mid = min + (max - min) / 2;
            if(nums[mid] == target) return mid;
       // min to mid sorted
            if(nums[min] <= nums[mid]) {
            // if target is within min and mid
                if(nums[min] <= target && target < nums[mid]) {
                    // remove right half because target is within min to mid
                    max = mid -1;
                } else {
                    min = mid + 1;
                }
            // mid to max sorted
            } else {
            // if target is within mid and  max
                if(nums[mid] < target && target <= nums[max]) {
                    min = mid + 1;
                } else {
                    max = mid - 1;
                }
            }
        }
            return -1;
    }
};
```
