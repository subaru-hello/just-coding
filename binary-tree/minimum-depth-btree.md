## 概要

https://leetcode.com/problems/minimum-depth-of-binary-tree/description/

> Given a binary tree, find its minimum depth.
> 
> 
> The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
> 
> **Note:** A leaf is a node with no children.
> 

## Intuition

子ノードの左右pointerが指す先が無い（null）状態になった最初のノードとrootとの距離を返す。

min関数を使えば良いというわけでもなさそう。

## Code

愚直にqueueを使ったBFS。FIFOだから広く探索を進めることができる。

```cpp
class Solution {
	public: 
				int minDepth(TreeNode* root) {
					// base case. if root has no leef, return 0 depth;
					if(root==NULL) return 0;
					// initialize TreeNode, depth queue.
					queue<pair<TreeNode*, int>>q;
					
					// set a first searching target to the queue
					q.push({root,1});
					
					// starts BFS
					while(!q.empty()) {
						auto [node,depth] = q.front();
						q.pop();
						if(node->left == NULL && node->right == NULL) {
							return depth;
						}
						
						if(node->left != NULL) {
							q.push({node->left,depth+1});
						}
						
						if(node->right != NULL) {
							q.push({node->right,depth+1});
						}
					}
						return 0;
				}
}
```

sync_with_stdioをfalseにするとI/Oスピードが上がるらしい。LeetCodeはコードがパスした時に他のユーザーの解答例が見れるから勉強になる。今回のケースの最速タイムのコードを書きに記す。

```cpp
class Solution {
public:
    int speed = []() {
        ios::sync_with_stdio(false);
        cin.tie(NULL);
        cout.tie(NULL);
        return 0;
    }();
    int minDepth(TreeNode* root) {
        if (!root)
            return 0;

        int left = minDepth(root->left);
        int right = minDepth(root->right);

        if (right == 0 || left == 0)
            return max (left, right) + 1;
        root->left = nullptr;
        root->right = nullptr;
        return min(left, right) + 1;
    }
};
```
