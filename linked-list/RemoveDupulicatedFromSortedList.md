### 問題

https://leetcode.com/problems/remove-duplicates-from-sorted-list

### 方針

Listの中身がemptyの場合は、dupulicatedになる余地がないのでheadをそのまま返す。

初回は、headの先頭を指すpointerを作る。

currかcurr→nextがNULLでない限りtraverseし続ける。

sortedなので、currとcurr→nextが同じ値の場合、dupulicateな値になっているので、返却するListに含めない。

### コード

Time Complexity O(N)

Space Complexity O(1)

初回

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
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL) return head;
        ListNode* curr = head;
        while(curr != NULL && curr -> next != NULL){
            if(curr->val == curr -> next -> val){
                ListNode* next = curr -> next-> next;
                delete(curr -> next);
                curr -> next = next;
            } else {
                curr = curr->next;
            }
        }
        return head;
    }
};
```

refined

比較している箇所を変数に入れることで、何をしているのかをわかりやすくした。

```cpp

class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL) return head;
        ListNode* curr = head;
        while(curr && curr -> next){
            bool isDupulicated = curr -> val == curr -> next -> val;
            if(!isDupulicated){
              curr = curr -> next;
              continue;
            }
            ListNode* next = curr -> next-> next;
            delete(curr -> next);
            curr -> next = next;
        }
        return head;
    }
};
```

コメントを入れた

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL) return head;
        ListNode* curr = head;
        while(curr && curr -> next){
            // 隣接Nodeと値が同じ場合は重複しているとみなす
            bool isDupulicated = curr -> val == curr -> next -> val;
            if(!isDupulicated){
                curr = curr -> next;
                continue;
            }
            // companctionさせることで重複要素を消す
            ListNode* twoAhead = curr -> next -> next;
            delete(curr->next);
            curr -> next = twoAhead;
        }
        return head;
    }
};
```
