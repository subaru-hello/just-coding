## 概要

https://leetcode.com/problems/maximum-depth-of-binary-tree/description/

> 
> 
> 
> Given the `root` of a binary tree, return *its maximum depth*.
> 
> A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.
> 

## Intuition

stackかqueue、recursiveかを選べる。TreeNodeが渡される時にすでに左と右nodeが作られているから容易に解ける。

## Complexity

O(N)

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
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        int leftDepth = maxDepth(root->left);
        int rightDepth = maxDepth(root->right);
        return 1 + max(leftDepth,rightDepth);
    }
};
```
