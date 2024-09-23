## 概要

https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/

> Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.
> 

- preorder

root→left→rightの順にNodeが入っていること

配列の最初の要素がrootであることを利用して、rootを取り出すために使う。

- inorder

left→root→rightの順にNodeが入っている。

vector[root-1]がleftである。

vector[root+1]がrightである。

- Binery Search Tree

BSTは

左が常にrootより小さい

右が常にrootより大きい

preorder traversal [9, 3, 15, 20, 7, 10, 39, 1, 94]

↓

```cpp
       9
      / \
     3   15
          \
           20
          /
         7
          \
          10
             \
             39
            /  \
           1    94

```

## Intuition

rootを取得して、

## Code

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    unordered_map<int, int> inorderMap;  // To store value -> index mapping for inorder traversal

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        // Build a hash map to store the index of each value in the inorder traversal
        for (int i = 0; i < inorder.size(); ++i) {
            inorderMap[inorder[i]] = i;
        }
        // Recursively construct the binary tree
        return buildTreeHelper(preorder, 0, preorder.size() - 1, 0, inorder.size() - 1);
    }
    
private:
    TreeNode* buildTreeHelper(vector<int>& preorder, int preStart, int preEnd, int inStart, int inEnd) {
        // Base case: if there are no elements to construct the tree
        if (preStart > preEnd || inStart > inEnd) {
            return nullptr;
        }
        
        // The first element of the preorder traversal is the root
        int rootVal = preorder[preStart];
        TreeNode* root = new TreeNode(rootVal);
        
        // Find the index of the root in the inorder traversal
        int rootIndexInInorder = inorderMap[rootVal];
        
        // Number of elements in the left subtree
        int leftSubtreeSize = rootIndexInInorder - inStart;
        
        // Recursively build the left subtree
        root->left = buildTreeHelper(preorder, preStart + 1, preStart + leftSubtreeSize, inStart, rootIndexInInorder - 1);
        
        // Recursively build the right subtree
        root->right = buildTreeHelper(preorder, preStart + leftSubtreeSize + 1, preEnd, rootIndexInInorder + 1, inEnd);
        
        return root;
    }
};

// Utility function to print the tree in-order (for validation)
void inorderPrint(TreeNode* root) {
    if (!root) return;
    inorderPrint(root->left);
    cout << root->val << " ";
    inorderPrint(root->right);
}

int main() {
    // Example usage
    vector<int> preorder = {3, 9, 20, 15, 7};  // Preorder traversal
    vector<int> inorder = {9, 3, 15, 20, 7};   // Inorder traversal
    
    Solution solution;
    TreeNode* root = solution.buildTree(preorder, inorder);
    
    // Print the tree to check if it's correctly constructed
    cout << "In-order traversal of the constructed tree: ";
    inorderPrint(root);  // Should match the input inorder traversal
    cout << endl;

    return 0;
}

```
