## [合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

> 将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

> ```
> 输入：l1 = [1,2,4], l2 = [1,3,4]
> 输出：[1,1,2,3,4,4]
> ```
>
> ```
> 输入：l1 = [], l2 = []
> 输出：[]
> ```
>
> ```
> 输入：l1 = [], l2 = [0]
> 输出：[0]
> ```



#### 自己想法

1. 小链表，easy

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if(list1 == NULL && list2 == NULL)
            return NULL;
        else if(list1 == NULL)
            return list2;
        else if(list2 == NULL)
            return list1;
        
        ListNode *p1 = list1;   //第一个点
        ListNode *p2 = list2;   
        ListNode *p = new ListNode(0);  //新的虚拟头结点
        ListNode *pp = p;   //副本一下
        while(p1!=NULL && p2!=NULL){
            if(p1->val < p2->val){  //p1小
                p->next = p1;
                p1 = p1->next;
                p = p->next;
            }
            else{
                p->next = p2;
                p2 = p2->next;
                p = p->next;
            }
        }
        if(p1 != NULL){
            p->next = p1;
        }
        if(p2 != NULL){
            p->next = p2;
        }
        return pp->next;
    }
};
```

