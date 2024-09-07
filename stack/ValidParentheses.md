## Problem Statement

https://leetcode.com/problems/valid-parentheses/description/

## Intuition

stackスペースを作成して、openが来たらpush、closeが来たらstackを見て対のopenがあればpop、なければfalseって感じで進めるのか

受け取る、openは優先して入れる、closeは中身を確認してから入れる、最後にstackがemptyか確認する、みたいな感じ。

## Approach

## Complexity

- Time complexity:
O(N)
- Space complexity:
O(1)

## Code

```cpp

class Solution {
public:
    bool isValid(string s) {
        // arange
        stack<char> pArr;
        for(char str : s) {
                if(str == '(' || str == '{' || str == '[') {
    pArr.push(str);
                } else {
                    if(pArr.empty() ||
                    (str == ')' && pArr.top() != '(') ||
                    (str == '}' && pArr.top() != '{') ||
                    (str == ']' && pArr.top() != '[') ) {
                        return false;
                    }
                    pArr.pop();
                }
            
        }
      return pArr.empty();
    }
};
```

より簡潔に書いた場合

```cpp
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char,char> matchingPair = {
            {')', '('},
            {']', '['},
            {'}', '{'}
        };

        stack<char> pareStack;
        for(char ch: s) {
            if(matchingPair.count(ch)){
                if(pareStack.empty() || pareStack.top() != matchingPair[ch]) {
                    return false;
                }
                pareStack.pop();
            } else {
                pareStack.push(ch);
            }
        }
        return pareStack.empty();
    }
};
```
