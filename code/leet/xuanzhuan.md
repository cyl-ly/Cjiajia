## [旋转链表](https://leetcode.cn/problems/rotate-list/)

> 给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动 `k` 个位置。

> ```
> 输入：head = [1,2,3,4,5], k = 2
> 输出：[4,5,1,2,3]
> ```
>
> ```
> 输入：head = [0,1,2], k = 4
> 输出：[2,0,1]
> ```



#### 题解

```c++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        //空间复杂度低：遍历循环链表，超时了，可以取余数次遍历，多余的遍历不用管
        //空间复杂度高：复制出来，循环完去建立
        if(k == 0)
            return head;
        if(head == NULL || head->next == NULL)
            return head;
        ListNode *p = head;
        int num = 0;
        while(p){
            num++;
            p = p->next;
        }
        k = k%num;  //只需要遍历余数次
        while(k--){     //至少存在两个节点了
            //进行一次向后推链表
            p = head;
            ListNode *pnext = p->next;
            int num1 = p->val;
            int num2 = 0;
            while(pnext->next){
                num2 = pnext->val;   
                pnext->val = num1;
                p = p->next;
                pnext = pnext->next;
                num1 = num2;
            }
            head->val = pnext->val;
            pnext->val = num1;
        }
        return head;
    }
};
```

