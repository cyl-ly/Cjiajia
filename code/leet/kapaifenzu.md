#### [卡牌分组](https://leetcode.cn/problems/x-of-a-kind-in-a-deck-of-cards/description/)

> 给定一副牌，每张牌上都写着一个整数。
>
> 此时，你需要选定一个数字 `X`，使我们可以将整副牌按下述规则分成 1 组或更多组：
>
> 每组都有 `X` 张牌。
>
> 组内所有的牌上都写着相同的整数。
>
> 仅当你可选的 `X >= 2` 时返回 `true`。

> ```
> 输入：deck = [1,2,3,4,4,3,2,1]
> 输出：true
> 解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]
> ```
>
> ```
> 输入：deck = [1,1,1,2,2,2,3,3]
> 输出：false
> 解释：没有满足要求的分组。
> ```



#### 自己想法

1. 暴力，找到最大公约数

```c++
class Solution {
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        vector<int> v1(10000);

        for(int i=0;i<deck.size();i++){
            v1[deck[i]]++;
        }

        auto min=min_element(v1.begin(),v1.end());
        auto max=max_element(v1.begin(),v1.end());

        bool flag=true;

        for(int i=2;i<=*max;i++){
            flag=true;

            for(int j=0;j<10000;j++){
                if(v1[j]%i!=0&&v1[j]!=0) 
                    flag=false;
            }
            if(flag)
                return true;
        }
        return false;
    }
};
```



#### 查看题解

1. 利用gcd()函数求最大公约数，需要初始化为第一个元素
2. unordered_map可以直接[]++

```c++
class Solution {
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        unordered_map<int,int> m1;
        for(auto i:deck)    m1[i]++;

        vector<int> cnt;
        for(auto i:m1) cnt.push_back(i.second);

        int g=cnt[0];   //初始化

        for(auto i:cnt){
            g=gcd(g,i);
        }

        return g>=2;
    }
};
```



#### 总结

1. `auto i=min_element()`返回的是迭代器

2. `vector<int> v1(100)`可以变换为`int c[100];` 然后非0复制到`vector`

3. `map`的遍历方法

   ```c++
   for(auto it : map1){			//C++11以后
   	cout << it.first <<" "<< it.second <<endl;
   }
   
   map<string, int>::iterator it;		//C++98
   for (it = m2.begin(); it != m2.end(); it++) {
       string s = it->first;
       printf("%s %d\n", s.data(), it->second);
   }
   ```

4. `map`找到最小值`value`

   ```c++
   bool cmp_value(const pair<int, int> left,const pair<int,int> right){
   	return left.second < right.second;
   }
   
   auto i= max_element(test.begin(),test.end(),cmp_value);
   ```

5. ```c++
   unordered_map<int, int> cnt;
   for(auto x : deck) cnt[x]++;
   for(auto i: cnt) vector.push_back(i.second);
   ```

6. `gcd()`函数，初始化

7. ```c++
   lcm=lcm*a[j]/gcd(lcm,a[j]); // 求最小公倍数
   ```

8. 只需要找到出现次数的最大公约数就可以，和代表数字没有关系