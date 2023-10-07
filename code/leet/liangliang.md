## [两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

> 给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

> ```
> 输入：head = [1,2,3,4]
> 输出：[2,1,4,3]
> ```
>
> ```
> 输入：head = []
> 输出：[]
> ```
>
> ```
> 输入：head = [1]
> 输出：[1]
> ```



#### 自己想法

1. 每跳转两次进行一次调整
2. 但是要注意后续转变时，因为当前节点顺序已经调整了，所以next时需要注意

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
    ListNode* swapPairs(ListNode* head) {
        if(head == NULL || head->next == NULL)
            return head;        //空或者只有一个节点的特殊情况
        
        ListNode* myhead = new ListNode(0, head);   //虚拟头结点
        ListNode* cnt = myhead->next->next;     //每跳两次进行一次调整
        ListNode* pre = myhead;
        while(cnt!=NULL){
            ListNode* p = cnt->next;    //防止断链，但是涉及到p->next时，需要确定p不是nullptr
            pre->next->next = p;
            cnt->next = pre->next;
            pre->next = cnt;
            
            pre = cnt->next;        //要注意，此时cnt已经调整了，不能直接的向下跳两步
            if(p == NULL)
                break;
            cnt = p->next;
        }
        return myhead->next;
    }
};
```

