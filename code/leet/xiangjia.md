## [两数相加II](https://leetcode.cn/problems/add-two-numbers-ii/description/)

> 给你两个 **非空** 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
>
> 你可以假设除了数字 0 之外，这两个数字都不会以零开头。

> ```
> 输入：l1 = [7,2,4,3], l2 = [5,6,4]
> 输出：[7,8,0,7]
> ```
>
> ```
> 输入：l1 = [2,4,3], l2 = [5,6,4]
> 输出：[8,0,7]
> ```
>
> ```
> 输入：l1 = [0], l2 = [0]
> 输出：[0]
> ```



#### 题解

1. 这个是逆序存储的，需要注意

```c++
class Solution {
public:
    ListNode* add(ListNode* l1, ListNode* l2) {
        //加法器
        int sum = 0;
        ListNode *p1 = l1;
        ListNode *p2 = l2;
        ListNode *head = new ListNode(-1, NULL);    //新结点
        ListNode *root = head;
        int flag = 0;   //进位标志
        while(p1 || p2){
            sum = flag == 0 ? 0 : 1;
            flag = 0;
            if(p1){
                sum += p1->val;
                p1 = p1->next;
            }
            if(p2){
                sum += p2->val;
                p2 = p2->next;
            }
            if(sum >= 10){
                flag = 1;
                sum = sum-10;
            } 
            ListNode *p = new ListNode(sum, NULL);
            head->next = p;
            head = p;
        }
        if(flag == 1){      //超出位数的进位
            ListNode *p = new ListNode(1, NULL);
            head->next = p;
        }
        return root->next;
    }
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //逆序存储，先反转链表，然后再相加，再反转？
        ListNode *p1 = new ListNode(-1, NULL);
        ListNode *p = l1;
        while(p){
            ListNode *r = p->next;
            p->next = p1->next;
            p1->next = p;
            p = r;
        }
        ListNode *p2 = new ListNode(-1, NULL);
        p = l2;
        while(p){
            ListNode *r = p->next;
            p->next = p2->next;
            p2->next = p;
            p = r;
        }
        p1 = add(p1->next, p2->next);   //p1是第一个节点
        p2->next =NULL;  //新虚拟节点
        p = p1;
        while(p){
            ListNode *r = p->next;
            p->next = p2->next;
            p2->next = p;
            p = r;
        }
        return p2->next;

    }
};
```

