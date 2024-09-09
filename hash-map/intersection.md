## 概要

https://leetcode.com/problems/intersection-of-two-arrays/

## Intuition

二つのvectorに共通する値をvectorで返却するアルゴリズム

同じvector内で共通する場合は無視する

別のvector間で共通する値をuniqueに返す

## Complexity

## Code

SetはC++でも使えるんだな。

https://cpprefjp.github.io/reference/set/set/insert.html

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        // build nums1 unique set
        unordered_set<int>num1Set(nums1.begin(), nums1.end());
        // build answer set
        unordered_set<int> ans;
        // iterate nums2 whether num1set has nums2 int valiable
        // use referencing(&) helps reducing total time because it does not copy nums2. Although it has a potential to modify nums2, nums2 is not be used later it's ok.
        for(int& x: nums2) {
            if(num1Set.count(x)){
                ans.insert(x);
            }
        }
        // return answer set converted into vector
        return vector<int>(ans.begin(),ans.end());
    }

};
```

より直感的な方

```jsx
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        // build nums1 unique set
        unordered_set<int>num1Set(nums1.begin(), nums1.end());
        unordered_set<int>num2Set(nums2.begin(), nums2.end());
        // build answer set
        vector<string> ans;
        // iterate nums2 whether num1set has nums2 int valiable
        // use referencing(&) helps reducing total time because it does not copy nums2. Although it has a potential to modify nums2, nums2 is not be used later it's ok.
        for(int& x: num1Set) {
            if(num2Set.count(x)){
                ans.insert(x);
            }
        }
        // return answer set converted into vector
        return ans;
    }

};
```
