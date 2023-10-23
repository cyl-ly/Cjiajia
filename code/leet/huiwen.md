## [回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

> 给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

> ```
> 输入：head = [1,2,2,1]
> 输出：true
> ```
>
> ```
> 输入：head = [1,2]
> 输出：false
> ```



#### 题解

1. 空间复杂度太高，还是ON

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        //用快慢指针找到中点，然后一半入栈，但是空间复杂度是ON
        
        if(head == NULL)
            return false;
        if(head->next == NULL)
            return true;

        ListNode *fast = head;
        ListNode *last = head;
        while(fast != NULL && fast->next != NULL){
            fast = fast->next->next;
            last = last->next;
        }
        
        if(fast != NULL)    //奇数个
            last = last->next;

        //先来个栈，一半去入栈
        stack<ListNode*> st;
        while(last != NULL){
            st.push(last);
            last = last->next;
        }

        ListNode *p = head;
        while(!st.empty()){
            last = st.top();
            if(last->val != p->val)
                return false;
            st.pop();
            p = p->next;
        }
        return true;
    }
};
```

2. 改进版，可以时间空间复杂度为O1，如果需要保持链表不变的话，还需要在最后再进行一次翻转链表

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        //用快慢指针找到中点，然后一半入栈，但是空间复杂度是ON

        if(head == NULL)
            return false;
        if(head->next == NULL)
            return true;

        ListNode *fast = head;
        ListNode *last = head;
        while(fast != NULL && fast->next != NULL){
            fast = fast->next->next;
            last = last->next;
        }
        if(fast != NULL)    //奇数个
            last = last->next;

        //反转后一半链表，头插入，去节约空间复杂度
        ListNode *p = last;
        ListNode *new_head = new ListNode(-1, NULL);

        while(p != NULL){
            ListNode *pp = p->next; //防止断开
            p->next = new_head->next;
            new_head->next = p;
            p = pp;
        }

        fast = head;    //做为另一个开始遍历
        last = new_head->next;
        while(last != NULL){
            if(fast->val != last->val)
                return false;
            fast = fast->next;
            last = last->next;
        }
        return true;
    }
};
```

