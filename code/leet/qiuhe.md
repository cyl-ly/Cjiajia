## [链表求和](https://leetcode.cn/problems/sum-lists-lcci/submissions/476893427/)

> 给定两个用链表表示的整数，每个节点包含一个数位。
>
> 这些数位是反向存放的，也就是个位排在链表首部。
>
> 编写函数对这两个整数求和，并用链表形式返回结果。

> ```
> 输入：(7 -> 1 -> 6) + (5 -> 9 -> 2)，即617 + 295
> 输出：2 -> 1 -> 9，即912
> ```



#### 题解

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
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
};
```

