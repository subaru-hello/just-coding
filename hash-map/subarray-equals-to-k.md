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

```cpp
sum(i,j) = prefix[j] − prefix[i−1]
prefix[i−1] = prefix[j] − k
```

> A prefix sum array, also known as a cumulative sum array, is a derived array that stores the cumulative sum of elements in a given array.
> 

渡された配列で、選択中の要素までの値を足し合わせた数を格納した配列のことを指す。

a given array: [1,2,5,6]

prefix sum: [1,3,8,14]

1. prefix sumを計算
2. ひとつ前のprefix sumをトラックするMapを作成
3. kと一致する、あるいは直前のMap値と足したらkと一致する場合

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        unordered_map<int, int> mp;
        vector<int> prefix_sum(n);
        prefix_sum[0] = nums[0];
        int ans = 0;

        // build a prefix_sum, starting with pre[1]
        for(int i = 1; i <n; ++i) {
            prefix_sum[i] = prefix_sum[i-1] + nums[i];
        }

        for(int i=0; i < n; ++i) {
            if(prefix_sum[i] -k == 0)
                ans++;
            // compare current prefix_sum - k iteratively.
            if(mp.find(prefix_sum[i] - k) != mp.end()){
              ans +=  mp[prefix_sum[i]-k];
            }
            mp[prefix_sum[i]]++;
        }
        return ans;
    }
};
```

mp.find(key)はkeyが見つかったらIteratorを返す

keyが見つからなかったらmp.end()を返す。

mp.end()は最後の要素を指すポインタのこと。

mp.find()で最後まで要素が見つからなければIteratorのポインタがMapの最後になるから、mp.find() == mp.end()は要素がない場合という意味になる。

ただ、mp.end()は番兵的な役割だから、Mapの最後の要素という意味は持たないことに注意。紛らわしい。。

例)

```cpp
#include <iostream>
#include <unordered_map>

int main() {
    std::unordered_map<int, int> mp = {{1, 100}, {2, 200}, {3, 300}};

    // Let's say the "last" element (in terms of insertion) is {3, 300}
    auto it = mp.find(3);  // Find the key '3'

    if (it != mp.end()) {
        std::cout << "Found key 3 with value: " << it->second << std::endl;
    } else {
        std::cout << "Key 3 not found." << std::endl;
    }

    return 0;
}
```

Output

```cpp
Found key 3 with value: 300
```
