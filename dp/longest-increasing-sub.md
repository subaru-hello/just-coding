## 概要

## Intuition

## Complexity

## Code

dpを使ったアルゴリズムはO(N*2)かかる

```cpp

```

```cpp
class Solution {
public:
       int lengthOfLIS(vector<int>& nums) {
            // subarray storage.
            vector<int> sub;
            // if it
            for (int x : nums) {
                if(sub.empty() || sub[sub.size() - 1] < x) {
                    sub.push_back(x);
                } else {
                    auto it = lower_bound(sub.begin(), sub.end(), x);
                    *it = x;
                }
            }
            return sub.size();

    }
};
```
