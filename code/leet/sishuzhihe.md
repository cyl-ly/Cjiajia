## [四数之和](https://leetcode.cn/problems/4sum/description/)

> 给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：
>
> - `0 <= a, b, c, d < n`
> - `a`、`b`、`c` 和 `d` **互不相同**
> - `nums[a] + nums[b] + nums[c] + nums[d] == target`
>
> 你可以按 **任意顺序** 返回答案 。

> ```
> 输入：nums = [1,0,-1,0,-2,2], target = 0
> 输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
> ```
>
> ```
> 输入：nums = [2,2,2,2,2], target = 8
> 输出：[[2,2,2,2]]
> ```



#### 自己想法

1. 一开始并不知道怎么去借鉴上一题的思路，想：把i和j的数组加起来当做一个数？那样没法去重啊，打开这个视频的一瞬间，想到了两层for遍历，然后直接去力扣实现自己的想法。i和j去重都是采用的上一题思路，这里就遇到了第一个问题，当下一层i遍历的时候，i的元素已经变化了，但是j和j-1可能会相同。后来经过思考，把条件变为了j-1>i，这样至少保证了i和j之间会空出距离，就会防止这种情况发生。遇到的第二个问题就是，直接不可能存在的情况，我是判断开头为正而且大于tar，一定不可能，结尾为负且小于tar也不可能，但是遇到了什么呢？遇到了相加超出int范围的数，只能就是用ll去计算，至此，本题完结

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        //三数是遍历i，+双指针
        //四个数求和的总数，是利用map统计次数，这里是输出组合
        //两层·for循环，
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());     //先排序
        if(nums[0] >0 &&nums[0] > target)
            return result;
        if(nums[nums.size()-1] < 0 && nums[nums.size()-1] < target)
            return result;
        
        for(int i=0; i<nums.size(); i++){
            if(i>0 && nums[i] == nums[i-1])
                continue;
            for(int j=i+1; j<nums.size(); j++){
                if(j-1>i && nums[j] == nums[j-1] )
                     continue;   //对j去重
                //怎么理解这里不会忽略后面不同left和right的情况呢
                //因为这是递增排序的，当j和前面的元素相同后，后面一定可以再次找到相同的left和right，
                //因为在大小地位相当于没有变化

                //出现了一个新的问题，就是新的一次遍历后，nums[i]不同了，但是j和j-1相同，[-2, -1, -1, 2]
                //用j-i>i之后，就可以排除下一轮的j和i元素相同导致的问题了，至少保证j和i空开了一个位置
                int left = j+1;
                int right = nums.size()-1;
                while(left<right){
                    long long num = nums[i]+nums[j];        //有个数字超了范围，只能用ll，但是四个int相加还不可以，必须分开加
                    num+=nums[left];
                    num+=nums[right];  
                    if(num < target){
                        left++;
                    }
                    else if(num > target){
                        right--;
                    }
                    else if(num == target){
                        result.push_back(vector<int>{nums[i], nums[j], nums[left], nums[right]});
                        while(left<right && nums[left] == nums[left+1])   left++;         //left和right进行去重
                        while(left<right && nums[right] == nums[right-1])     right--;
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
};
```

