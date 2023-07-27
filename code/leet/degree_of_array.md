### 数组的度

> 给定一个非空且只包含非负数的整数数组 nums，数组的 度 的定义是指数组里任一元素出现频数的最大值。
>
> 你的任务是在 nums 中找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度
>
> ```
> 输入：nums = [1,2,2,3,1]
> 输出：2
> 解释：
> 输入数组的度是 2 ，因为元素 1 和 2 的出现频数最大，均为 2 。
> 连续子数组里面拥有相同度的有如下所示：
> [1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
> 最短连续子数组 [2, 2] 的长度为 2 ，所以返回 2
> ```
>
> ```
> 输入：nums = [1,2,2,3,1,4,2]
> 输出：6
> 解释：
> 数组的度是 3 ，因为元素 2 重复出现 3 次。
> 所以 [2,2,3,1,4,2] 是最短子数组，因此返回 6
> ```



[自己思考](#自己思考)

[查看题解](#查看题解)

[总结](#总结)



## 自己思考

> 执行用时：852 ms, 在所有 C++ 提交中击败了5.02% 的用户
>
> 内存消耗：24.6 MB, 在所有 C++ 提交中击败了78.94% 的用户

```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {   
        if(nums.size()==1)
            return 1;
        map<int,int> a;     //存放数字及其出现次数

        for(int i=0;i<nums.size();i++){
            if(a.count(nums[i]) > 0){       //存在这个key了
                ++a[nums[i]];               //对应value+1
            else
                a.insert(map<int,int>::value_type(nums[i],1));      //插入键值对
        }
        int max=0;		//记录最大的度
        for(auto &e:a){
            if(e.second>max)
                max=e.second;       //遍历，找到最大值
        }

        vector<int> v1;				//存储度相同的元素
        for(auto &e:a){
            if(e.second==max){
                v1.push_back(e.first);      //根据value存储key
                cout<<e.first<<endl;
            }
        }

        bool flag1,flag2;		//是否找到头尾的标志
        int num1,num2;          //标记度的数组下标
        int max1=INT_MAX;
        for(int i=0;i<v1.size();i++){       //开始遍历每个度的元素
            flag1=false;    //每个度的元素都重置
            flag2=false;

            for(int j=0;j<nums.size();j++){
                if(v1[i]==nums[j]&&flag1==false){
                    flag1=true;         //找到了头，后续不再改变
                    num1=j;     
                }
                if(v1[i]==nums[nums.size()-j-1]&&flag2==false){      //对称从末位开始找
                    flag2=true;
                    num2=nums.size()-j-1;
                }
            }
            max1=min(num2-num1+1,max1);
        }
        return max1;
    }
};
```

1. 第一时间就没有想到可以用什么特殊的结构来简便一下，只是常规繁琐的思路
2. 先用map存储元素和出现次数，然后找到最大的度，再遍历一次，把所有最大度对应的key找到，存储到vector中
3. 依次相同最大度的元素，去遍历nums，找到最先出现的头和尾，比较返回就可以了
4. 思路很常规，导致运行时间很长



## 查看题解

1. `unordered_map`常用于查找问题，内部实现哈希表

   `map`内部实现的红黑树，查找，插入和删除的复杂度都是`O(logn)`,因此效率非常高，但是空间占用率高

   > 执行用时：36 ms, 在所有 C++ 提交中击败了63.09% 的用户
   >
   > 内存消耗：24.8 MB, 在所有 C++ 提交中击败了74.21% 的用户

   ```c++
   class Solution {
   public:
       int findShortestSubArray(vector<int>& nums) {
           unordered_map<int, vector<int>> map;   
           //unordered_map是一个将key和value关联起来的容器
           
           int max=INT_MIN;    //记录最大的度
           int max_num=0;
           for(int i=0;i<nums.size();i++){
               if(map.count(nums[i])>0){      //已经出现过了
                   map[nums[i]][0]++;          //[nums[i]]是key，[0]是出现次数
                   map[nums[i]][2]=i;          //[2]是最后一次出现的位置
               }else{
                   map[nums[i]]={1, i ,i};    //第一次出现，次数，位置，位置
               }
               if(map[nums[i]][0]>max){         //较大的度出现
                   max=map[nums[i]][0];        
                   max_num=nums[i];            //保存数字
               }else if(map[nums[i]][0]==max && map[nums[i]][2]-map[nums[i]][1]<map[max_num][2]-map[max_num][1]){
               //相等的度出现，并且你的首末位置比现在的小，就替换
                   max_num=nums[i];
               }
            }
           return map[max_num][2]-map[max_num][1]+1;
       }
   };
   ```

   通过这个题解，知道了`map`还可以关联`int`和`vector`，从而可以存储更多的关联因素

2. 滑动窗口

   ```c++
   class Solution {
   public:
       int findShortestSubArray(vector<int>& nums) {
           unordered_map<int, int> mp;		// 定义统计哈希表和最大频数
           int max_fre = 0;
   
           for(int i = 0; i < nums.size(); i ++) {
               mp[nums[i]] ++;		//对应位置次数增加			
               max_fre = max(max_fre, mp[nums[i]]);	//一次遍历就把度找出来了
           } 
           mp.erase(mp.begin(), mp.end());
           // 定义窗口和满足频数的最短长度
           int ans = nums.size();
           int left = 0, right = 0;
           
           while(right < nums.size()) {
               mp[nums[right]] ++;			//这我咋看不懂了呢？ mp已经重置了，++是啥作用
               // 频数达到要求后，移动左边界
               while(mp[nums[right]] == max_fre) {		//窗口大小等于
                   ans = min(ans, right - left + 1);
                   mp[nums[left ++]] --;
               }
               right ++;
           }
           return ans;
       }
   };
   ```

   

## 总结

1. map补充的相关操作

   ```c++
   m.count(key);		//存在返回1
   m.find(key);		//返回的是指向键值为key的元素，如果没有找到就是返回尾部的迭代器
   ++m[nums[i]];		//可以增加value值
   for(auto &e:m){		//key和value的另一种表示
       cout<<e.first<<" "<<e.second<<endl;
   }
   ```

2. vector操作

   ```c++
   int max = *max_element(v1.begin(), v1.end());       //vector找到最大值
   int num = find(v1.begin(),v1.end(),max)-v.begin();	//find返回迭代器，减去begin就是下标
   ```

3. `unordered_map<int,vector<int>> map;`的相关表示，操作

