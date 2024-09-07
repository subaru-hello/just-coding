## 問題

https://leetcode.com/problems/add-two-numbers/

## Intuition

二つの配列の、同じ位を足し合わせて、出来上がったLinkedListをそのままかえすような計算をする。

10を超えた数を表現するために、1になるキャリーフラグをつくる。

計算中の位を10で除算したあまりをその位の数にする

## Code

```cpp
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode();
        ListNode* tail = dummy;
        int carry = 0;

        while(l1 != nullptr || l2 != nullptr || carry !=0){
           int l1Digit = (l1 != nullptr) ? l1 -> val : 0;
           int l2Digit = (l2 != nullptr) ? l2-> val : 0;
           int sum = l1Digit + l2Digit + carry;
           int digit = sum % 10; 
            carry = sum /10;

            ListNode* newNode = new ListNode(digit);
            tail-> next = newNode;
            tail = tail -> next;

            l1 = (l1 != nullptr) ? l1 -> next : 0;
            l2 = (l2 != nullptr) ? l2 -> next : 0;
        }

        ListNode* result = dummy -> next;

        return result;
    }
};
```
