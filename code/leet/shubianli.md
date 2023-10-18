## [二叉树的遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

> 给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

> ```
> 输入：root = [1,null,2,3]
> 输出：[1,3,2]
> ```



#### 自己想法

1. **中序非递归遍历**，一路向西，入栈，p=p左，else出栈，p=p右

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if(root == NULL)
            return result;
        TreeNode *p = root;
        while(p!=NULL || !st.empty()){
            if(p!=NULL){
                st.push(p);
                p = p->left;
            }
            else{
                p = st.top();
                st.pop();
                result.push_back(p->val);
                p = p->right;
            }
        }
        return result;
    }
};
```

2. **前序非递归**，步步为营，先右后左孩子

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;  //入栈的是指针
        vector<int> result;
        if(root==NULL)
            return result;
        st.push(root);      //入栈前面一定要判断是否符合条件
        
        while(!st.empty()){
            TreeNode *p = st.top();
            st.pop();
            result.push_back(p->val);
            if(p->right!=NULL)
                st.push(p->right);
            if(p->left!=NULL)
                st.push(p->left);
        }
        return result;
    }
};
```

3. **后序非递归**，前序变孩子，reverse

```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;  //入栈的是指针
        vector<int> result;
        if(root==NULL)
            return result;
        st.push(root);      //入栈前面一定要判断是否符合条件
        while(!st.empty()){
            TreeNode *p = st.top();
            st.pop();
            result.push_back(p->val);
            if(p->left!=NULL)           //先左后右
                st.push(p->left);
            if(p->right!=NULL)
                st.push(p->right);
        }
        reverse(result.begin(), result.end());      //进行颠倒
        return result;
    }
};
```

4. **递归遍历**

```c++
class Solution {
public:
    void pretree(TreeNode* cur, vector<int>& v){       //返回类型和参数 
        //要想修改，就需要&引用
        if(cur == NULL)     //终止条件
            return ;
        v.push_back(cur->val);		//前序遍历
        pretree(cur->left, v);      //递归条件
        pretree(cur->right, v);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        pretree(root, result);
        return result;
    }
};

//pretree(cur->left, v);  
//v.push_back(cur->val);		//中序遍历
//pretree(cur->right, v);
```

