## 概要

## Code

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
       if(wordDict.empty()) return false;
       // stringにもsizeが使える
       int n = s.size();
       vector<bool> dp(n+1, false);
       unordered_set<string> wordSet(wordDict.begin(),wordDict.end());
        // Base Case: 0 is no word segmenting -> true;
        dp[0] = true;
        for (int i = 1; i <= n; i++) {
            // find whether segmented or not in each iteration until j reaches to s length
            for(int j = 0; j < n; j++) {
                if(dp[j] && wordSet.find(s.substr(j, i-j))!=wordSet.end()) {
                    // set segmented point to true
                    dp[i] = true;
                    break;
                }
            }
        }
        // whether all wordDict can split the last position of the s;
    return dp[n];
 }
};
```
