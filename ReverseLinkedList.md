# Intuition

# Approach
前、今、次の３つのnodeを作って、前後のポインタをnullptrに達するまで付け替え続ける。

# Complexity
- Time complexity:
O(N)

- Space complexity:
O(1)

# Code
```cpp []
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* next = nullptr;
        ListNode* curr = head;
        while(curr != nullptr){
  	    // 現在地点になる予定のポインタを保持しておく
            next = curr -> next;
	    //  付け替え
            curr -> next =  prev;
	    // 前と現在の付け替え
            prev = curr;
            curr = next;
        }
        // return head of the list
        return prev;
    }
};
```
