## [删除链表的倒数第n个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

> 给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

> ```
> 输入：head = [1,2,3,4,5], n = 2
> 输出：[1,2,3,5]
> ```
>
> ```
> 输入：head = [1], n = 1
> 输出：[]
> ```
>
> ```
> 输入：head = [1,2], n = 1
> 输出：[1]
> ```

> - 链表中结点的数目为 `sz`
> - `1 <= sz <= 30`
> - `0 <= Node.val <= 100`
> - `1 <= n <= sz`



#### 自己想法

1. 快慢指针即可

```c++
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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //快慢指针
        //要注意，这里是删除倒数第n个，要注意快慢的间隔
        //涉及了删除操作，要进行delete

        ListNode* cnt = new ListNode(0, head);      //虚拟结点
        ListNode* slow = cnt;
        ListNode* fast = cnt;
        int num=0;  //快慢指针间隔计数器
        while(fast!=NULL){
            fast = fast->next;      //移动快指针
            num++;
            if(num > n+1){
                slow = slow->next;  //移动慢指针，移动完之后，slow是倒数第n个的前驱
            }
        }
        ListNode* t = slow->next;
        slow->next = slow->next->next;
        delete t;
        return cnt->next;
    }
};
```

