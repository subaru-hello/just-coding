> 
> 
> 
> Given a string s, find the length of the **longest substring**
> 
> without repeating characters.
> 

https://leetcode.com/problems/longest-substring-without-repeating-characters/description/

## Intuition

２つのポインターを使って最長のsubstringを探していく。

ここは、Sliding Windowアルゴリズムを活用する。

substringとsubsequenceの違いは、consecutive orderedかunconsecutive but orderedにある。

abcdefという文字がある時、abcやbcefは順に連続する文字列なのでsubstringになる。aceやbdfは順だが非連続な文字なのでsubsequenceになる。

今回は、abcやbcefのようなsubstringになる重複のない文字列長を探す問題ということになる。

重複、という文言が出たら大体setが出てくる。

Sliding Windowは、ある範囲を追う時に使えるアルゴリズムだと解釈した。今回で言うと、重複のない連想コンテナを追っている。

与えられた範囲の先端と終端をpointerで追い、setコンテナの長さを返す。

## Code

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // two pointers to keep track of right char and left char
       int right = 0, left = 0, maxLength = 0;
       unordered_set<char> charSet;
        // 最後の要素に達するまで
        while(right < s.size()) {
            // 新しい文字の場合
            if(charSet.find(s[right]) == charSet.end()) {
                charSet.insert(s[right]);
                maxLength = max(maxLength, right - left + 1);
                right++;
            } else {
                charSet.erase(s[left]);
                left++;
            }
        }
        // return max between set size or pointer index
        return maxLength;
    }
};
```
