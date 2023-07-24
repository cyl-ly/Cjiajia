> 假设有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花不能种植在相邻的地块上，它们会争夺水源，两者都会死去。
>
> 给你一个整数数组 flowerbed 表示花坛，由若干 0 和 1 组成，其中 0 表示没种植花，1 表示种植了花。另有一个数 n ，能否在不打破种植规则的情况下种入 n 朵花？能则返回 true ，不能则返回 false 。
>
> ```
> 输入：flowerbed = [1,0,0,0,1], n = 1
> 输出：true
> ```
>
> ```
> 输入：flowerbed = [1,0,0,0,1], n = 2
> 输出：false
> ```

[自己想法](#自己想法)

[查看题解](#查看题解)

[总结](#总结)

## 自己想法

> 执行用时：20 ms, 在所有 C++ 提交中击败了24.60% 的用户
>
> 内存消耗：20.9 MB, 在所有 C++ 提交中击败了5.06% 的用户

```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        vector<int> v1;
        int num=0;      //第几个0片段
        bool flag=false;
        int nn=0;       //当前片段有几个0
        
        for(int e=0;e<flowerbed.size();e++){

            if(flowerbed[e]==0&&e==flowerbed.size()-1){	//结尾是0
                nn++;
                v1.push_back(nn);
                nn=0;
            }
            else if(flowerbed[e]==0&&flowerbed[e+1]==0){	//00组合，继续遍历
                nn++;
            }
            else if(flowerbed[e]==0&&flowerbed[e+1]==1){	//01组合，存储前0片段
                nn++;
                v1.push_back(nn);
                nn=0;
            }
            else if(flowerbed[e]==1){	//1，存储本身
                v1.push_back(nn);
                nn=0;
            }
        }
        int sum=0;
        if(v1.size()==1){			//全是0
            sum=(v1[0]+1)/2;		//单独判断
        }
        else 
            sum=v1[0]/2+v1[v1.size()-1]/2;	//首尾的0片段单独判断
        for(int e=1;e<v1.size()-1;e++){
            sum+=(v1[e]-1)/2;				//中间正常的0片段
        }
        if(n<=sum)
            return true;
        return false;
    }
};
```

1. 遍历数组，依次找到连续的0片段，~~个数大于3就能放一个，个数大于5就能放两个，依次类推~~

   ~~就是，(n-1)/2就是能放入花的个数，然后再多个片段个数相加~~，实践证明，这个行不通

2. 又换了一种方式存储，一个`vector`来存储遇到0片段中0的个数和1（1对应位置存储0）

3. [1,0,0,0,1,0]对应存储[0,3,0,1]，然后整体计算sum的值，和n比较就能得出结果

4. 注意的点

   1. 开头和末位的0片段，计算sum和中间的片段不一样
   2. 如果只有一个片段，也需要单独计算

## 查看题解

1. 评论区想法

   还是我自己1.当时的想法，竟然可以实现，但是需要额外处理，尊的值得学习

   ```c++
   class Solution {
   public:
       bool canPlaceFlowers(vector<int>& flowerbed, int n) {
           int sum=0;
           int num=1;
           //中间的0片段插花个数的公式统一，但是首尾的0不一样
           //前后端补0，插画个数的公式就和中间的一样了
           for(int e:flowerbed){
               if(e==0){
                   num++;
               }
               else{       //遇到1，结算上个区间
                   sum+=(num-1)/2;
                   num=0;
               }
           }
           //如果0区间是结尾，那么还没有结算
           num++;      //后端补0
           sum+=(num-1)/2;
           if(n<=sum)
               return true;
           return false;
       }
   };
   ```

   > 执行用时：16 ms, 在所有 C++ 提交中击败了58.94% 的用户
   >
   > 内存消耗：19.7 MB, 在所有 C++ 提交中击败了86.84% 的用户

2. 官方题解，贪心算法？