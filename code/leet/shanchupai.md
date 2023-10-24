## [删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/submissions/476796174/)

> 给定一个已排序的链表的头 `head` ， *删除所有重复的元素，使每个元素只出现一次* 。返回 *已排序的链表* 。

> ```
> 输入：head = [1,1,2]
> 输出：[1,2]
> ```
>
> ```
> 输入：head = [1,1,2,3,3]
> 输出：[1,2,3]
> ```



#### 题解

1. 两种思路

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        //不管空间复杂度的话，可以复制到数组去重，然后再去返回
        //需要顾及空间复杂度的话，就去一遍遍历，删除

        if(head == NULL || head->next == NULL)
            return head;
        
        ListNode *p = head;
        ListNode *p_next = p->next;     //至少两个节点，肯定不会空
        while(p_next){
            if(p_next->val == p->val){
                p->next = p_next->next;
                p_next = p->next;
            }
            else{
                p = p->next;
                p_next = p_next->next;
            }
        }
        return head;
    }
};
```

