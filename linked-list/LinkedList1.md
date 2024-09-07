https://leetcode.com/problems/linked-list-cycle/description/

## 問題

> 
> 
> 
> Given `head`, the head of a linked list, determine if the linked list has a cycle in it.
> 
> There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.
> 
> Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.
> 

## 方針

同じ場所を２回訪れた場合循環があると言える。

headからスタートする遅いpointerと、headの次を指している速いpointerを用意。速いpointerが最後まで遅いpointerと出会わずに終了した場合、cycleが無い。

## code

Two Pointer

time complexity: O(N)

space complexity: O(1)

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
    bool hasCycle(ListNode *head) {
			ListNode* slow = head;
			ListNode* fast = head -> next;

			if(head == NULL || head -> next == NULL){
			    return false;
			}

			while(slow != fast){
			   if(fast -> next == NULL || fast -> next -> next == NULL){
			     return false;
			   }

				slow = slow -> next;
			  fast = fast -> next -> next;
			}

		return true;
	}
};

```

### 別の解法

訪れたListNodeのポイントする先を逆にしていく(reverse pointers)

time complexity: O(N)

space complexity: O(1)

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
    bool hasCycle(ListNode *head) {
        ListNode* current = head;
        ListNode* prev = NULL;
        while(current){
            ListNode* tmp_current = current -> next;
            current -> next = prev;
            prev = current;
            current = tmp_current;
            if(current == head){
                return true;
            }
        }
        return false;
    }
};
```

### reference

[Floyd’s Tortoise And Hare(亀兎)](https://www.notion.so/Floyd-s-Tortoise-And-Hare-dc05df292c564b578430697ed2535030?pvs=21)
