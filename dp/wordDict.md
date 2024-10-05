## 概要

https://leetcode.com/problems/word-break/

> Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.
> 
> 
> **Note** that the same word in the dictionary may be reused multiple times in the segmentation.
> 

## Intuition

与えられるinputを確認する

セグメンティング対象のstringとセグメントするワード配列。

条件を確認する。

- ワード配列には重複した文字がある可能性がある
- 渡されない可能性がある
- 

方針として、

- セグメントするワードがsのサブストリングと一致するかを１字づつトラバースする
- 一致する位置をdpベクターに保存
- dpベクターの最後がtrueだったら全てをセグメントかできる

ような実装をする

## Code

O(N^2)

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
       if(wordDict.empty()) return false;
       // stringにもsizeが使える
       int n = s.size();
       vector<bool> dp(n+1, false);
       unordered_set<string> wordSet(wordDict.begin(),wordDict.end());
        // if dict is an empty, it is true;
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
