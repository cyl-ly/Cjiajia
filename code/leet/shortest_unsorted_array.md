> 给你一个整数数组 nums ，你需要找出一个 连续子数组 ，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。
>
> 请你找出符合题意的 最短 子数组，并输出它的长度。
>
> ```
> 输入：nums = [2,6,4,8,10,9,15]
> 输出：5
> 解释：你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序
> ```
>
> ```
> 输入：nums = [1,2,3,4]
> 输出：0
> ```
>
> ```
> 输入：nums = [1]
> 输出：0
> ```

[自己想法](#自己想法)

[查看题解](#查看题解)

[总结](#总结)

## 自己想法

执行用时：24 ms, 在所有 C++ 提交中击败了75.96% 的用户
内存消耗：26.9 MB, 在所有 C++ 提交中击败了7.31% 的用户

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        vector<int> v1(nums);
        sort(v1.begin(),v1.end());			//直接排序，和排好序不同的就是需要调整的

        int i,j;
        for(int e=0;e<nums.size()-1;e++){	//确定下界
            if(v1[e]!=nums[e]){
                i=e;
                break;						//退出for
            }
        }
        for(int e=nums.size()-1;e>0;e--){	//确定上界
            if(v1[e]!=nums[e]){
                j=e;
                break;
            }
        }
        if(i-j+1==nums.size())		//全都一样，特殊判断
            return 0;
        return j-i+1;			//如果合为一起，用? : 时间不变，但占用内存会变大
    }
};
```

- 最开始，并没有什么头绪，想的是从头遍历，遇到比自己小的就标记作为i，从尾遍历，遇到比自己大的就标记作为j，但是只符合一小部分案例
- 而后想到了vector的排序功能，这个排序思想在上一题《第三大的数字》已经用到过了，没有第一时间想到，确实不应该。排好序列之后，和原序列对比，不一样的，我们就要调整
- 排序后，就头尾分别遍历，找到不一样的，就进行标记，最后判断一下特殊情况就可以了
- 要注意，头遍历的时候，要`e<size()-1`这个点，因为下标`size()`的话，就已经出去了，这个点之前遍历的时候竟然没有注意到，在打印`i,j`的时候才发现



## 查看题解

1. 题解一，这个和我的思想基本一致，但是运行时间很长，内存近似，是因为while循环的运行时间和for的有区别吗？？

   ```c++
   class Solution {
   public:
       int findUnsortedSubarray(vector<int>& nums) {
           if (is_sorted(nums.begin(), nums.end())) {
               return 0;
           }
           vector<int> numsSorted(nums);
           sort(numsSorted.begin(), numsSorted.end());
           int left = 0;
           while (nums[left] == numsSorted[left]) {
               left++;
           }
           int right = nums.size() - 1;
           while (nums[right] == numsSorted[right]) {
               right--;
           }
           return right - left + 1;
       }
   };
   ```

2. 题解二

   这个思路和我最开始的想法差不多，或者说差很多

   我是左到右遍历，确定调整的左界，右到左遍历，确定调整的右界

   但是`1,2,3,1,11,9,10`这种情况就不适用，因为现阶段的边界，有可能后面还会比他大或者小

   题解是，左到右遍历，确定右界，反之相同

3. 思路

   左到右，记录已经遍历的max，遇到一次小于max的就记为右边界，**注意遍历并不会停止**，直到遍历完毕才可以，相反同理

   ```c++
   class Solution {
   public:
       int findUnsortedSubarray(vector<int>& nums) {
           int n = nums.size();
           int maxn = INT_MIN, right = -1;
           int minn = INT_MAX, left = -1;
           for (int i = 0; i < n; i++) {
               if (maxn > nums[i]) {
                   right = i;				//遇到小的，就记录end，但是还接着遍历
               } else {
                   maxn = nums[i];			//遇到大的，更新max
               }
               if (minn < nums[n - i - 1]) {	//利用下标，找到对称的位置，只一次遍历
                   left = n - i - 1;
               } else {
                   minn = nums[n - i - 1];
               }
           }
           return right == -1 ? 0 : right - left + 1;
       }
   };
   ```

## 总结

1. vector的自带排序
2. 数组一次遍历，前后同时调整，`[i]`和`[n-i-1]`
3. `vector`未声明大小与声明大小的时候，`push_back`插入的位置在整体数据中的提现是有区别的。以及未声明大小空间的时候，只有`push_back`后，才能使用下标进行访问