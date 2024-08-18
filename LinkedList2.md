Last login: Wed Aug 14 22:04:59 on console
subaru@wajis-Air ~ % ls
>
public:
                        }

Applications
CertificateSigningRequest.certSigningRequest
Desktop
Dev
Documents
Downloads
Library
Movies
Music
Pictures
Postman
Postman Agent
Public
cloud-builders-community
development
go
subaru@wajis-Air ~ % cd Dev
subaru@wajis-Air Dev % ls
Wanpo		hugo-blog
subaru@wajis-Air Dev % mkdir arai60
subaru@wajis-Air Dev % cd arai60
subaru@wajis-Air arai60 % ls
subaru@wajis-Air arai60 % echo "# just-coding" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:subaru-hello/just-coding.git
git push -u origin main
Initialized empty Git repository in /Users/subaru/Dev/arai60/.git/
[main (root-commit) 5235218] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
Enumerating objects: 3, done.
### 問題
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 226 bytes | 226.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:subaru-hello/just-coding.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
subaru@wajis-Air arai60 % ls
README.md
subaru@wajis-Air arai60 %
subaru@wajis-Air arai60 %
subaru@wajis-Air arai60 %
subaru@wajis-Air arai60 % ls
README.md
subaru@wajis-Air arai60 % vim LinkedList1.md
subaru@wajis-Air arai60 % gs
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	LinkedList1.md

nothing added to commit but untracked files present (use "git add" to track)
subaru@wajis-Air arai60 % ga .
subaru@wajis-Air arai60 % gs
On branch main
Your branch is up to date with 'origin/main'.

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
```~
~
~
~
~
~
~
~
~
~
~
~
~
~
"LinkedList2.md" 28L, 965B
