## [快乐数](https://leetcode.cn/problems/happy-number/)

> 编写一个算法来判断一个数 `n` 是不是快乐数。
>
> **「快乐数」** 定义为：
>
> - 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
> - 然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。
> - 如果这个过程 **结果为** 1，那么这个数就是快乐数。
>
> 如果 `n` 是 *快乐数* 就返回 `true` ；不是，则返回 `false` 。

> ```
> 输入：n = 19
> 输出：true
> 解释：
> 12 + 92 = 82
> 82 + 22 = 68
> 62 + 82 = 100
> 12 + 02 + 02 = 1
> ```
>
> ```
> 输入：n = 2
> 输出：false
> ```



#### 自己想法

1. 怎么判断他是失败的，这点很重要

```c++
class Solution {
public:
    int mul(int n){
        int sum = 0;
        while(n){
            sum += (n%10)*(n%10);
            n = n/10;
        }
        return sum;
    }
    bool isHappy(int n) {
        //如果sum得到了1，那就直接true
        //怎么判断失败，就是这个sum已经出现过了，那就是已经进入循环了
        unordered_set<int> cnt;
        while(1){
            int sum = mul(n);
            if(cnt.find(sum) == cnt.end()){
                cnt.insert(sum);        //没出现过，就插入进去
                n = sum;
            }
            else if(sum == 1)
                return true;
            else
                return false;
        }
        return true;
    }
};
```

