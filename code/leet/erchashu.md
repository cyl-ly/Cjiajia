## [二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

> 给你二叉树的根结点 `root` ，请你将它展开为一个单链表：
>
> - 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
> - 展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/先序遍历/6442839?fr=aladdin) 顺序相同。

> ```
> 输入：root = [1,2,5,3,4,null,6]
> 输出：[1,null,2,null,3,null,4,null,5,null,6]
> ```



#### 题解

1. 空间复杂度高

```c++
class Solution {
public:
    void preorder(TreeNode *t, vector<int> &nums){
        if(t == NULL)
            return;
        //1
        nums.push_back(t->val);
        preorder(t->left, nums);
        //2
        preorder(t->right, nums);
        //3
    }
    void flatten(TreeNode* root) {
        //空间复杂度高：先序收集遍历顺序，再去生成一个树去建立
        //空间复杂度低：
        if(root != NULL){
            vector<int> order;
            preorder(root, order);
        
            TreeNode *p = root;
            p->left = NULL;
            for(int i=1; i<order.size(); i++){
                TreeNode *pp = new TreeNode(order[i], NULL, NULL);
                p->right = pp;
                p = p->right;
            }
        }
    }
};
```

2. 空间复杂度低，循环，移动挂载节点

```c++
class Solution {
public:
    void flatten(TreeNode* root) {
        //空间复杂度低：当前节点的右子树，挂到左子树的最右节点上

        TreeNode *head = root;
        while(head){
            if(head->left){
                TreeNode *p = head->left;
                while(p->right)
                    p = p->right;   //p到了左子树的最右节点
                
                p->right = head->right;     //把当前节点的右子树挂到左子树的最右节点上
                head->right = head->left;       //整体移动到右子树
                head->left = NULL;          //断开左子树
            }
            head = head->right;         //向右遍历下一个节点
        }
    }
};
```

