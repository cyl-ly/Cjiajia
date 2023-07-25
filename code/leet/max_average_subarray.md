> 给你一个由 n 个元素组成的整数数组 nums 和一个整数 k 。
>
> 请你找出平均数最大且 长度为 k 的连续子数组，并输出该最大平均数。
>
> 任何误差小于 10-5 的答案都将被视为正确答案
>
> ```
> 输入：nums = [1,12,-5,-6,50,3], k = 4
> 输出：12.75
> 解释：最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
> ```
>
> ```
> 输入：nums = [5], k = 1
> 输出：5.00000
> ```

## 自己想法

> 超时！！！！

```c++
//#include <numeric>
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double sum=INT_MIN,num=INT_MIN;    
        vector<int>::iterator it=nums.begin();
        vector<int>::iterator end=it+k;
        while(end<=nums.end()){    
            sum=accumulate(it++,end++,0);      
            if(sum>num)
                num=sum;
        }
        return num/k;  
    }
};

```

1. 就是一个滑动窗口，边计算，边比较，边滑动，但是通过了124/127，后面三个几w的数据超时了，可能是迭代器比较费时间？？
2. 但思路不难，谁想到，最简单的数据累加竟然时间没事，我还以为用了一个`accumulate`函数会给我加快呢
3. 要注意一下就是`accumulate`函数的`end`也是最后一个元素的下一个迭代器，第三个0是初始的累加和

## 查看题解

没啥看的，就变成数组就可以了，气啊

```c++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double sum=0;
        for(int i=0;i<k;i++)
            sum+=nums[i];

        double num=sum;
        for(int i=k;i<nums.size();i++){
            sum=sum-nums[i-k]+nums[i];
            num=max(sum,num);     
            //if(sum>num)     //时间长
            //    num=sum;
        }
        return num/k;
    }
};
```

## 总结

1. `accumulate(begin,end,0)`参数
2. `max(num,sum)`