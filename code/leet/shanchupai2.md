## [删除排序链表中的重复元素II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/submissions/476804158/)

> 给定一个已排序的链表的头 `head` ， *删除原始链表中所有重复数字的节点，只留下不同的数字* 。返回 *已排序的链表* 。

> ```
> 输入：head = [1,2,3,3,4,4,5]
> 输出：[1,2,5]
> ```
>
> ```
> 输入：head = [1,1,1,2,3]
> 输出：[2,3]
> ```



#### 题解

1. 现在空间复杂度比较高

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        //不考虑空间复杂度，可以去复制到哈希表，然后根据value值进行重分配
        //考虑空间复杂度，应该是需要三个指针去进行遍历把

        if(head == NULL || head->next == NULL)
            return head;
        
        ListNode *p = head;     //至少两个节点
        unordered_map<int, int> mp;
        while(p){
            mp[p->val]++;
            p = p->next;
        }
        ListNode *n = new ListNode(-1, head);   //虚拟头结点
        p = n;
        while(p->next){
            if(mp[p->next->val] == 1){
                p = p->next;
            }
            else{
                p->next = p->next->next;    //p保持不动，next更新了
            }
        }
        return n->next;
    }
};
```

2. 空间复杂度降低了

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        //不考虑空间复杂度，可以去复制到哈希表，然后根据value值进行重分配
        //考虑空间复杂度，应该是需要三个指针去进行遍历把

        if(head == NULL || head->next == NULL)
            return head;
        
        ListNode *p = new ListNode(-1, head);   //虚拟头结点
        ListNode *result = p;
        while(p->next && p->next->next){
            int val = p->next->val;     //先保存值
            if(p->next->next->val == val){      //出现重复元素了
                while(p->next && p->next->val == val)   //循环删除
                    p->next = p->next->next;
            }
            else
                p = p->next;
        }
        return result->next;
    }
};
```

