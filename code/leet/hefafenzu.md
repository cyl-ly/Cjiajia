## [合法分组的最少组数](https://leetcode.cn/problems/minimum-number-of-groups-to-create-a-valid-assignment/)

> 给你一个长度为 `n` 下标从 **0** 开始的整数数组 `nums` 。
>
> 我们想将下标进行分组，使得 `[0, n - 1]` 内所有下标 `i` 都 **恰好** 被分到其中一组。
>
> 如果以下条件成立，我们说这个分组方案是合法的：
>
> - 对于每个组 `g` ，同一组内所有下标在 `nums` 中对应的数值都相等。
> - 对于任意两个组 `g1` 和 `g2` ，两个组中 **下标数量** 的 **差值不超过** `1` 。
>
> 请你返回一个整数，表示得到一个合法分组方案的 **最少** 组数。

> ```
> 输入：nums = [3,2,3,2,3]
> 输出：2
> 解释：一个得到 2 个分组的方案如下，中括号内的数字都是下标：
> 组 1 -> [0,2,4]
> 组 2 -> [1,3]
> 所有下标都只属于一个组。
> 组 1 中，nums[0] == nums[2] == nums[4] ，所有下标对应的数值都相等。
> 组 2 中，nums[1] == nums[3] ，所有下标对应的数值都相等。
> 组 1 中下标数目为 3 ，组 2 中下标数目为 2 。
> 两者之差不超过 1 。
> 无法得到一个小于 2 组的答案，因为如果只有 1 组，组内所有下标对应的数值都要相等。
> 所以答案为 2 。
> ```
>
> ```
> 输入：nums = [10,10,10,3,1,1]
> 输出：4
> 解释：一个得到 2 个分组的方案如下，中括号内的数字都是下标：
> 组 1 -> [0]
> 组 2 -> [1,2]
> 组 3 -> [3]
> 组 4 -> [4,5]
> 分组方案满足题目要求的两个条件。
> 无法得到一个小于 4 组的答案。
> 所以答案为 4 。
> ```



#### 题解

1. 枚举每次分组的k即可。主要a/b向上取整的写法
2. map遍历的时候不要用下标i，因为map[i]这样也会创建元素，应该直接用迭代器it.second是value

```c++
class Solution {
public:
    int minGroupsForValidAssignment(vector<int>& nums) {
        if(nums.size() == 1)
		    return 1;

	    unordered_map<int,int> p;
	    int result = INT_MAX;
        int minnum = INT_MAX;
	    for(auto i:nums)
		    p[i]++;     //次数

	    for(auto i:p)
		    minnum = min(minnum, i.second);     //找到最小值
	    
	    for(int k=1; k<=minnum; k++){
		    //每次分成k和k+1，进行遍历
		    int sum = 0;
		    for(auto i : p){
			    int num1 = i.second/k; //直接分的组数，这里直接用p[i]会创建新的
			    int num2 = i.second%k; //余数
			    if(num2 <= num1 || num1 == 0){   //余数可以把他加到那些组上
				    sum += (i.second+k)/(k+1);  
                    //a/b向上取整的方法
			    }
			    else{
				    sum = INT_MAX;
				    break;
			    }
		    }
		    result = min(result, sum);
	    }
	    return result == INT_MAX ? -1 : result;
    }
};
```

