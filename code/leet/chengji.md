#### [构建乘积数组](https://leetcode.cn/problems/gou-jian-cheng-ji-shu-zu-lcof/description/)

> 给定一个数组 `A[0,1,…,n-1]`，请构建一个数组 `B[0,1,…,n-1]`，其中 `B[i]` 的值是数组 `A` 中除了下标 `i` 以外的元素的积, 即 `B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]`。不能使用除法。

> ```】
> 输入: [1,2,3,4,5]
> 输出: [120,60,40,30,24]
> ```



#### 自己想法

1. 暴力，复制n份，内存超时。遍历两层，时间超时

```c++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        vector< vector<int> > cnt;
        vector<int> b;      //小容器
        int n=a.size();
        for(int i=0;i<n;i++){
            b=a;
            b[i]=1;
            cnt.push_back(b);
        }

        vector<int> vt;
        for(int i=0;i<n;i++){
            int num=1;
            for(int j=0;j<n;j++){
                num=num*cnt[i][j];
            }
            vt.push_back(num);
        }
        return vt;
    }
};
```



#### 查看题解

1. 分别用left和right代表左右两个累计数组的乘积，每次只需要再乘一个新的元素就可以了
2. 看到评论说，这个一次遍历，但是一次里面访问了i和n-i-1，内存会频繁调入调出，和两次遍历是一样的效果

```c++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        int n=a.size();
        vector<int> b(n,1);

        int left=1,right=1;

        for(int i=0;i<n;i++){
            b[i]*=left;				//当前*左边的累计
            left*=a[i];				//更新左边元素的累计乘积
        
            b[n-i-1]*=right;
            right*=a[n-i-1];
        }
        return b;
    }
};
```



#### 总结

1. 用到左边累计乘积的概念