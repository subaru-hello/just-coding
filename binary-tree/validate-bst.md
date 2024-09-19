## 概要

https://leetcode.com/problems/validate-binary-search-tree/description/

> Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.
> 
> 
> A **valid BST** is defined as follows:
> 
> - The left  of a node contains only nodes with keys **less than** the node's key.
>     
>     subtree
>     
> - The right subtree of a node contains only nodes with keys **greater than** the node's key.
> - Both the left and right subtrees must also be binary search trees.

## Intuition

BSTは子が同じ数の場合を想定しないといけない。

この実装だと、離れたsubtreeがとBSTを実現できているかの判定ができない

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
    bool isValidBST(TreeNode* root) {
        // use queue
        if(!root) return false;
        // push root to queue
        queue<TreeNode*>q;
        // ans bool
        q.push(root);
        // traverse queue whether right val is greater than root, the other is smaller
        while(!q.empty()) {
            int size = q.size();
            TreeNode* node = q.front();
            q.pop();
            if(node->right) {
                if(node->right->val < node->val) return false;
                q.push(node->right);
            }
            if(node->left) {
                if(node->left->val >= node->val) return false;
                q.push(node->left);
            }
        }

        // unless it has leaf push to the queue.
        // if leaf return true;
        return true;
    }
};
```

## Code

recursiveに解いていく

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
    bool isValidBST(TreeNode* root) {
            return helper(root, nullptr, nullptr);
    }

    bool helper(TreeNode* node, TreeNode* minNode, TreeNode* maxNode) {
        if(!node) return true;

        if(minNode && node->val <= minNode->val) return false;
        if(maxNode && node->val >= maxNode->val) return false;

        return (helper(node->left, minNode, node) && helper(node->right, node, maxNode));
    }
};
```
