## 概要

https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/

## Intuition

普通のTraversalじゃなく、途中で順番を変えるロジックを付け足す。

前回の順序を保持するstateを持たなければならない。

## Code

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(!root) return ans;
        queue<TreeNode*> q;
        bool zigzag = false;
        q.push(root);
        while(!q.empty()) {
            int size = q.size();
            vector<int> currentVal;
            for(int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            currentVal.push_back({node->val});
                if(node->left) {
                    q.push(node->left);
                }
                if(node->right) {
                    q.push(node->right);
                }
            }
            if(zigzag) reverse(currentVal.begin(), currentVal.end());
            ans.push_back(currentVal);
            zigzag = !zigzag;
        }
        return ans;
    }
};
```
