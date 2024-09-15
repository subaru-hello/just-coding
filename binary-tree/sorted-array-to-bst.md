https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/

## Intuition

Binery Search Treeを作る実装は、quick sortに似ている。

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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBSTHelper(nums, 0, nums.size() -1);
    };

private:
   TreeNode* sortedArrayToBSTHelper(vector<int>& nums, int left, int right) {
        if(left > right) return nullptr;
        int pivot = left + (right - left) / 2;
        TreeNode* root = new TreeNode(nums[pivot]);
        root -> left = sortedArrayToBSTHelper(nums, left, pivot -1);
        root -> right = sortedArrayToBSTHelper(nums, pivot + 1, right);
        return root;
   }
};
```
