## [滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/submissions/474785792/)

> 给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。
>
> 返回 *滑动窗口中的最大值* 。

> ```
> 输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
> 输出：[3,3,5,5,6,7]
> 解释：
> 滑动窗口的位置                最大值
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```
>
> ```
> 输入：nums = [1], k = 1
> 输出：[1]
> ```



#### 自己想法

1. 单调队列，保证优先获取最大值

```c++
class Solution {
private:
    class mydeque{
    //利用双端队列实现单调队列
    //对外接口pop push getmax
    //每次插入元素的时候，要保证“当前始终”是最大的，其余的从队尾弹走
    //当要pop最大值的时候，从前面弹走
    public:
        deque<int> md;
        void pop(int key){
            if(!md.empty() && key == md.front())
                md.pop_front();     //要弹走的元素是最大值，才会在前面弹出
        }
        void push(int key){
            while(!md.empty() && md.back() < key)
                md.pop_back();      //插入时，比自己小的后面弹走
            md.push_back(key);
        }
        int getmax(){
            return md.front();
        }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        mydeque cnt;
        vector<int> result;
        for(int i=0; i<k; i++)
            cnt.push(nums[i]);
        result.push_back(cnt.getmax());
        for(int i=k; i<nums.size(); i++){
            cnt.pop(nums[i-k]);
            cnt.push(nums[i]);
            result.push_back(cnt.getmax());
        }
        return result;
    }
};
```

