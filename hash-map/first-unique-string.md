## 概要

https://leetcode.com/problems/first-unique-character-in-a-string/description/

## Intuition

ASCII文字列の引き算をKey, 回数をValueにしたHash Mapを作成。

HashMapをiterateして、最初にValueが1になった値を出力

26個の個数、渡された文字を先頭から読み込んで、最初に１になったものが最初のユニークな文字になるというアルゴリズム。

Initialize a vector size of 26 with all zeros(the amount of lower characters)

Iterate through each character in the string `"leetcode"`:

- For `c = 'l'`:
    
    `char_count['l' - 'a'] = char_count[11]`
    
    Increment `char_count[11]` by `1`:
    
    `char_count[11]` becomes `1`.
    
- For `c = 'e'`:
    
    `char_count['e' - 'a'] = char_count[4]`
    
    Increment `char_count[4]` by `1`:
    
    `char_count[4]` becomes `1`.
    
- For `c = 'e'` again:
    
    Increment `char_count[4]` by `1`:
    
    `char_count[4]` becomes `2`.
    

## Complexity

- for each character c, this operation takes O(1)

        for(char c: s) {
            lower_chars[c-'a']++;
        }

- this operation also takes O(N)

        for(int i = 0; i < s.size(); ++i) {
            if(lower_chars[s[i] - 'a'] == 1) {
                return i;
            }
        }

so, overall, it takes O(N)

## Code

```jsx
class Solution {
public:
    int firstUniqChar(string s) {
        vector<int> lower_chars(26,0);
        for(char c: s) {
            lower_chars[c-'a']++;
        }

        for(int i = 0; i < s.size(); ++i) {
            if(lower_chars[s[i] - 'a'] == 1) {
                return i;
            }
        }
        return -1;
    }
};
```
