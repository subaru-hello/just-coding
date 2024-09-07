### 問題

> Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.
> 
> 
> There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.
> 
> **Do not modify** the linked list.
> 

### 方針

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/d754495e-9e0f-4a9f-90a3-d9033bf94c16/443a5061-e9b1-4730-b9e2-6e798b86b392/image.png)

Floyd Tutoise and Hayeを使って、cycleを探す。

兎が先に最後尾まで達した(NULL)場合、cycleが無いということなのでNULLを返す

兎が亀と出会ったらcycleがある。

出会ったと時に亀のポインタをheadに戻して、再度出会った地点がcycleのスタートポイントになる。

### Code

Time  Complexity: O(N)
Space Complexity: O(1)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;
        while(fast && fast -> next){
            fast = fast -> next -> next;
            slow = slow -> next;
            if(fast == slow) break;
        }
        if(!(fast && fast -> next)) return NULL;

        while(head != slow){
            head = head -> next;
            slow = slow -> next;
        }
        return head;

    }
};
```