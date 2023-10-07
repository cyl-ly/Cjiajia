#### [移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

链表的定义

```c++
// 单链表
struct ListNode {
    int val;  // 节点上存储的元素
    ListNode *next;  // 指向下一个节点的指针
    ListNode(int x) : val(x), next(NULL) {}  // 节点的构造函数
};

ListNode* head = new ListNode(5);
```

> 给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

> ```
> 输入：head = [1,2,6,3,4,5,6], val = 6
> 输出：[1,2,3,4,5]
> ```
>
> ```
> 输入：head = [], val = 1
> 输出：[]
> ```
>
> ```
> 输入：head = [7,7,7,7], val = 7
> 输出：[]
> ```



#### 自己想法

1. 可以自己添加虚拟头结点，保证后续所有的操作都是一样的
2. 只有根据头结点才能找到一个完整的单链表
3. `while(p->next!=NULL)`前面还需要`&&p!=NULL`
4. 始终要知道删除节点的前驱，所以要用index->next进行遍历

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
    ListNode* removeElements(ListNode* head, int val) {
        //不能用头结点去遍历，因为我们最后还要返回这个链表
        //前面加了一个空节点作为头结点，这样后面所有节点删除操作都是一样的
        ListNode* myhead = new ListNode(0, head);
        ListNode* p = myhead;

        while(p!=NULL && p->next != NULL){
            if(p->next->val == val){
                ListNode* t = p->next;
                p->next = p->next->next;
                delete t;
            }
            else{
                p = p->next;
            }
        }
        return myhead->next;
    }
};
```



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
    ListNode* removeElements(ListNode* head, int val) {
        //始终要知道删除节点的前驱，所以要用index->next进行遍历

        ListNode* cnt = head;
        while(cnt!=NULL && cnt->val == val){    //删除头结点
            ListNode* t = cnt;
            cnt = cnt->next;
            delete t;
        }

        ListNode* index = cnt;

        while(index!=NULL && index->next!=NULL){
            if(index->next->val == val){
                ListNode*t = index->next;
                index->next = index->next->next;
                delete t;
            }
            else{
                index = index->next;
            }
        }
        return cnt;
        
    }
};
```

