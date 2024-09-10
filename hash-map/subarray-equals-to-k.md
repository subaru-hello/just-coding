## 概要

https://leetcode.com/problems/subarray-sum-equals-k/description/

## Intuition

two sum algorithmに似ている気がする。hash-mapに分類されているということは、heapではなく、hash mapで解く問題ということだろう。

同じindexの数字は使えない

emptyはない。

## 計算量

## Code

Brute Forceでゴリっとやるとこうなる

https://leetcode.com/problems/subarray-sum-equals-k/solutions/1759909/c-full-explained-every-step-w-dry-run-o-n-2-o-n-two-approaches/

```cpp
class Solution {
public:
    int subarraySum(vector<int>& arr, int k) {
        int n = arr.size(); // taking the size of the array
        
        int ans = 0; // ans variable to store our count
        
        for(int i = 0; i < n; i++) // traverse from the array
        {
            int sum = arr[i]; // provide sum with arr[i]
            
            if(sum == k) // if element itself equal to k, then also increment count
                ans++;
            
            for(int j = i + 1; j < n; j++) // now moving forward,
            {
                sum += arr[j]; // add elements with sum
                
                if(sum == k) // if at any point they become equal to k
                    ans++; // increment answer
            }
            
        }
        
        return ans; // and at last, return answer
    }
};
```

prefix sumを使用する。

> A prefix sum array, also known as a cumulative sum array, is a derived array that stores the cumulative sum of elements in a given array.
> 

渡された配列で、選択中の要素までの値を足し合わせた数を格納した配列のことを指す。

a given array: [1,2,5,6]

prefix sum: [1,3,8,14]

```cpp

```
