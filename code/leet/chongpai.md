## [重排链表](https://leetcode.cn/problems/reorder-list/description/)

> 给定一个单链表 `L` 的头节点 `head` ，单链表 `L` 表示为：
>
> ```
> L0 → L1 → … → Ln - 1 → Ln
> ```
>
> 请将其重新排列后变为：
>
> ```
> L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
> ```
>
> 不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

> ```
> 输入：head = [1,2,3,4]
> 输出：[1,4,2,3]
> ```
>
> ```
> 输入：head = [1,2,3,4,5]
> 输出：[1,5,2,4,3]
> ```



#### 题解

1. 要记得，改变链表的时候要断开才行

```c++
class Solution {
public:
    void reorderList(ListNode* head) {
        ListNode *fast = head;
        ListNode *last = head;
        while(fast && fast->next){
            last = last->next;
            fast = fast->next->next;
        }
        //if(fast){
            fast = last->next;
            last->next = NULL;
            last = fast;
        //}   //偶数个时没能断开链表，找到规律，偶数个时，不需要平分中点

        ListNode *root = new ListNode(-1, NULL);
        //倒序last后面
        while(last){
            fast = last->next;
            last->next = root->next;
            root->next = last;
            last = fast;
        }
        //两个表交叉连接
        ListNode *p1 = head;
        ListNode *p2 = root->next;
        while(p2){//p2短
            ListNode *r1 = p1->next;
            ListNode *r2 = p2->next;
            
            p2->next = p1->next;
            p1->next = p2;

            p1 = r1;
            p2 = r2;
        }
    }
};
```

