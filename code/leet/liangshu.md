## [两数相加](https://leetcode.cn/problems/add-two-numbers/description/)

> 给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。
>
> 请你将两个数相加，并以相同形式返回一个表示和的链表。
>
> 你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

> ```
> 输入：l1 = [2,4,3], l2 = [5,6,4]
> 输出：[7,0,8]
> 解释：342 + 465 = 807.
> ```



#### 自己想法

1. 简陋版进位相加，但是整理时发现，性能差别不大，就是代码格式难看一点

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //暴力会超范围，直接采用十进制去做

        ListNode *p1 = l1;
        ListNode *p2 = l2;
        ListNode *result = new ListNode(0, NULL);   //虚拟头结点
        ListNode *copy = result;    //复制品
        while(p1!=NULL && p2!=NULL){
            int num = p1->val+p2->val;
            while(num >= 10){
                if(p1->next)    //进制
                    p1->next->val++;
                else if(p2->next)
                    p2->next->val++;
                else{
                    ListNode *n = new ListNode(1, NULL);    //超进位了
                    p1->next = n;   //加入到p1
                }
                num -= 10;
            }
            ListNode *r = new ListNode(num, NULL);
            copy->next = r;
            copy = r;
            p1 = p1->next;
            p2 = p2->next;
        }
        if(p1!=NULL){
            ListNode *r = p1;   //位置标记
            //另一个链表空了，这时第一个元素很可能会超出10
            while(p1!=NULL){
                if(p1->val >= 10){
                    p1->val -= 10;
                    if(p1->next)
                        p1->next->val++;
                    else{
                        ListNode *n = new ListNode(1, NULL);    //超进位了
                        p1->next = n;   //加入到p1
                    }
                }
                p1 = p1->next;
            }
            copy->next = r;
        }
        if(p2!=NULL){
            ListNode *r = p2;   //位置标记
            //另一个链表空了，这时第一个元素很可能会超出10
            while(p2!=NULL){
                if(p2->val >= 10){
                    p2->val -= 10;
                    if(p2->next)
                        p2->next->val++;
                    else{
                        ListNode *n = new ListNode(1, NULL);    //超进位了
                        p2->next = n;   //加入到p1
                    }
                }
                p2 = p2->next;
            }
            copy->next = r;
        }
        if(result->next)
            return result->next;
        return NULL;
    }
};
```

2. 进阶版进位相加

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *result = new ListNode(-1);    //虚拟头
        ListNode *copy = result;
        int sum = 0;    //用它去收集每位的和，加入到新链表
        int flag = 0;   //上一位的进位标志
        while(l1!=NULL || l2!=NULL){
            if(flag == 1)
                sum = 1;
            else
                sum = 0;    
            flag = 0;       //重置
            if(l1!=NULL){
                sum += l1->val;
                l1 = l1->next;
            }
            if(l2!=NULL){
                sum += l2->val;
                l2 = l2->next;
            }
            if(sum >= 10){
                sum -= 10;
                flag = 1;
            }
            ListNode *p = new ListNode(sum);
            copy->next = p; //连接
            copy = p;
        }
        if(flag == 1){//链表都没了，但是还有个进位
            ListNode *n = new ListNode(1, NULL);
            copy->next = n;
        }
        return result->next!=NULL ? result->next:NULL;
    }
};
```

