> 
> 
> 
> Given an array nums
> 
> of distinct integers, return all the possible
> 
> permutations you can return the answer in **any order**
> 

https://leetcode.com/problems/permutations/description/

Permutation: SPIとかでよくみる2P1とかでお馴染みのP

## Intuition

base caseまでたどり着いたら一つステップを戻すか終了するアルゴリズムである、backtrackを使用する。

グラフの幅優先探索はサイクルが含まれる可能性があ離、複数回探索されてしまうことを防ぐためにvisitというフラグを用いていた。

Permutationも再帰を用いるため、探索済みの数字はスキップするようusedを使う。

## Code

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
     vector<int> current;
     vector<bool> used(nums.size(), false);
     backtrack(nums,current,result,used);
     return result;
        }

    /* 
        nums = 渡された全ての数
        current = 探索中の配列
        rest = 結果配列
        used = 探索済みを判定するフラグ
    */
    void  backtrack(vector<int>& nums, vector<int>& current,vector<vector<int>>& rest,vector<bool> used) {
        // 最後まで探索済みの場合は終了
        if(current.size() == nums.size()) {
            rest.push_back(current);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if(used[i]) continue;
            used[i] = true;
            current.push_back(nums[i]);
            backtrack(nums, current, rest, used);
            // 以下がbacktrackの特徴 やり直す処理
            used[i] = false;
            current.pop_back();
        }
    }
};
```
