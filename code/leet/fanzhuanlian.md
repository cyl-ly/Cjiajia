## [反转链表II](https://leetcode.cn/problems/reverse-linked-list-ii/)

> 给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

> ```
> 输入：head = [1,2,3,4,5], left = 2, right = 4
> 输出：[1,4,3,2,5]
> ```



#### 题解

1. 细节很多，一不注意就很多错误的地方

```c++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if(left == right)
            return head;
        int num = right-left+1;
        ListNode *begin = head;
        ListNode *new_head = new ListNode(-1, NULL);
        
        while(--left){  //new_head为头，begin是开始反转的起点
            new_head = begin;
            begin = begin->next;
        }
        if(left != 1)
            new_head->next = NULL;  //等于1的话，newhead就作为虚拟头姐点
        

        while(num != 0 && begin != NULL){
            ListNode *p = begin->next;
            begin->next = new_head->next;
            new_head->next = begin;
            begin = p;
            num--;
        }
        ListNode *p = new_head;
        while(p->next != NULL){
            p = p->next;
        }
        p->next = begin; //连接老链表
        return new_head->val == -1 ? new_head->next : head; 
        //left 1时就要返回虚拟头结点，不是1的话head就没有变动
    }
};
```

