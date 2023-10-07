## [设计链表](https://leetcode.cn/problems/design-linked-list/)

> 你可以选择使用单链表或者双链表，设计并实现自己的链表。
>
> 单链表中的节点应该具备两个属性：`val` 和 `next` 。`val` 是当前节点的值，`next` 是指向下一个节点的指针/引用。
>
> 如果是双向链表，则还需要属性 `prev` 以指示链表中的上一个节点。假设链表中的所有节点下标从 **0** 开始。
>
> 实现 `MyLinkedList` 类：
>
> - `MyLinkedList()` 初始化 `MyLinkedList` 对象。
> - `int get(int index)` 获取链表中下标为 `index` 的节点的值。如果下标无效，则返回 `-1` 。
> - `void addAtHead(int val)` 将一个值为 `val` 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
> - `void addAtTail(int val)` 将一个值为 `val` 的节点追加到链表中作为链表的最后一个元素。
> - `void addAtIndex(int index, int val)` 将一个值为 `val` 的节点插入到链表中下标为 `index` 的节点之前。如果 `index` 等于链表的长度，那么该节点会被追加到链表的末尾。如果 `index` 比长度更大，该节点将 **不会插入** 到链表中。
> - `void deleteAtIndex(int index)` 如果下标有效，则删除链表中下标为 `index` 的节点。

> ```
> 输入
> ["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
> [[], [1], [3], [1, 2], [1], [1], [1]]
> 输出
> [null, null, null, null, 2, null, 3]
> 
> 解释
> MyLinkedList myLinkedList = new MyLinkedList();
> myLinkedList.addAtHead(1);
> myLinkedList.addAtTail(3);
> myLinkedList.addAtIndex(1, 2);    // 链表变为 1->2->3
> myLinkedList.get(1);              // 返回 2
> myLinkedList.deleteAtIndex(1);    // 现在，链表变为 1->3
> myLinkedList.get(1);              // 返回 3
> ```



#### 自己想法

1. 自己声明类内成员，以及链表的结构体，构造函数
2. 虚拟节点本身就是成员，不用再次声明，只需要给他开辟空间就可以
3. 往后移动找节点时，尽量用for
4. 删除操作时，释放内存

```c++
class MyLinkedList {
private:
    int size;
    LinkedNode* myhead;     //虚拟头结点

    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
        LinkedNode(int val,LinkedNode* next):val(val),next(next){}//两个参数
    };
public:
    MyLinkedList() {        //生成虚拟头结点
        this->myhead = new LinkedNode(0);   //myhead本身就是自带的数据结构，不需要再次声明了，只给他开辟空间就可以了
        this->size = 0;     //类内
    }
    
    int get(int index) {
        if(index < 0 || index > size-1) //不合法下标
            return -1;
        LinkedNode* p = myhead->next;
        for(int i= 0; i<index; i++)         //往后寻找时，尽量用for，用while时先--和后--区别很大
            p = p->next;
        return p->val;
    }
    
    void addAtHead(int val) {
        LinkedNode* cnt = new LinkedNode(val, myhead->next);    //声明新节点
        myhead->next = cnt;
        size++;
    }
    
    void addAtTail(int val) {
        LinkedNode* p = new LinkedNode(val);
        LinkedNode* cur = myhead;
        while(cur->next!=NULL){
            cur = cur->next;
        }
        cur->next = p;
        size++;
    }
    
    void addAtIndex(int index, int val) {
        if(index == size)       //特殊情况
            addAtTail(val);
        else if(index>=0 && index<=size-1){   //下标有效
            LinkedNode* p = new LinkedNode(val);      //新结点
            LinkedNode* cur = myhead;
            for(int i=0; i<index; i++)
                cur = cur->next;
            p->next = cur->next;
            cur->next = p;
            size++;
        }
    }
    
    void deleteAtIndex(int index) {
        if(index>=0 && index<=size-1){   //下标有效
            LinkedNode* cur = myhead;
            for(int i=0; i<index; i++)
                cur = cur->next;
            LinkedNode* p = cur->next;
            cur->next = cur->next->next;
            delete p;
            size--;
        }
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

