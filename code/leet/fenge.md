## [分隔链表](https://leetcode.cn/problems/partition-list/)

> 给你一个链表的头节点 `head` 和一个特定值 `x` ，请你对链表进行分隔，使得所有 **小于** `x` 的节点都出现在 **大于或等于** `x` 的节点之前。
>
> 你应当 **保留** 两个分区中每个节点的初始相对位置。

> ```
> 输入：head = [1,4,3,2,5,2], x = 3
> 输出：[1,2,2,4,3,5]
> ```
>
> ```
> 输入：head = [2,1], x = 2
> 输出：[1,2]
> ```



#### 题解

1. 左神思想改造，引入了虚拟头结点，讨论更方便

```c++
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        //维护两个区域的链表，并选择虚拟头结点
        
        ListNode *lbegin = new ListNode(-1);
        ListNode *lend = lbegin;
        ListNode *hbegin = new ListNode(-1);
        ListNode *hend = hbegin;

        ListNode *p = head;
        while(p){
            ListNode *r = p->next;
            if(p->val < x){
                lend->next = p;
                lend = p;
            }
            else{
                hend->next = p;
                hend = p;
            }
            p = r;
        }
        hend->next = NULL;  //在虚拟头结点的情况下，这种情况都考虑到了
        lend->next = hbegin->next;
        return lbegin->next;
    }
};
```

