## [反转字符串2](https://leetcode.cn/problems/reverse-string-ii/description/)

> 给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每计数至 `2k` 个字符，就反转这 `2k` 字符中的前 `k` 个字符。
>
> - 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
> - 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

> ```
> 输入：s = "abcdefg", k = 2
> 输出："bacdfeg"
> ```
>
> ```
> 输入：s = "abcd", k = 2
> 输出："bacd"
> ```



#### 自己想法

1. 无非就是循环翻转

```c++
class Solution {
public:
    void reverseString(string& s, int begin, int end) {   //都是下标
        int i = begin;
        int j = end; 
        while(i<=j){
            swap(s[i++], s[j--]);
        }
    }
    string reverseStr(string s, int k) {
        int n = s.size();
        int num1 = n/(2*k);         //有多少个整数2k
        int num2 = n%(2*k);         //余数是多少
        int begin = 0;
        for(int i=0; i<num1; i++){  //i为循环次数
            reverseString(s, begin, begin+k-1);
            begin = begin+2*k;
        }     
        if(num2 < k)
            reverseString(s, begin, n-1);
        else
            reverseString(s, begin, begin+k-1);
        return s;
    }
};
```

