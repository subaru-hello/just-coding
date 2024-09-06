## 概要

https://leetcode.com/problems/group-anagrams/description/

## Intuition

bloom filterを使ってアナグラムを判定できるかも？と思った

mapをうまく活用する。

for(auto x: strs){}はマップにも使えることを知った

## Code

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        // initialize an unordered_map
        unordered_map<string, vector<string>> mp;
        // build the map with setting sorted strs key and strs[i] value
        for(auto x: strs) {
            string word = x;
            sort(word.begin(), word.end());
            mp[word].push_back(x);
        }
        vector<vector<string>> ans;
       // push each map values in the map to the vector contains answer in iteration.
      for(auto x: mp) {
            ans.push_back(x.second);
       }
        return ans;
    }
};
```
