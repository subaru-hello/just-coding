## 問題

https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii

## 方針

重複した場合、現在のNodeと次のNodeを削除するという点がbasicと違う。

dummy nodeを使って、headを消してもheap use after free errorが出るのを予防する。

dummyを使わないでhead nodeを削除した場合、アドレスを解法した後に利用することはできないみたいなエラーが出る。

```cpp
Line 57: Char 28:
=================================================================
==22==ERROR: AddressSanitizer: heap-use-after-free on address 0x502000000098 at pc 0x55cc7f2a7cd9 bp 0x7ffed88524d0 sp 0x7ffed88524c8
READ of size 8 at 0x502000000098 thread T0
```

### 効率

Time O(N)

Space O(1)

## 解法

順序立てて解法を組み立てると、いい感じにチャンクがてきて覚えやすい。

dummy nodeをheadをpointさせて作る

prev nodeをdummyにpointさせて作る

現在のnode(head)に値があって次のnodeと同じ値の場合は重複してるので、重複しなくなるまでskip

それ以外はprevとheadを次に進める

headがない場合はdummyの次を返す。

dummyはheadを指しているので、headが返ることになる

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
        ListNode* dummy = new ListNode(0,head);
        ListNode* prev = dummy;
        while(head){
            // 下のwhileと冗長な書き方だが、それぞれ参照しているheadが違うため共通変数に括っていない
            if(head->next && head->val == head->next->val){
                while(head->next && head->val== head->next->val){
                    head = head -> next;
                }
                head = head->next;
                // prevの次だったはずのポインタが変わっているのでheadに更新する
                prev ->next = head;
            }else{
                head = head -> next;
                // prevの次はheadのため、上のようにはしていない。
                prev = prev->next;
            }
        }
        return dummy -> next;
            
    }
};
```
