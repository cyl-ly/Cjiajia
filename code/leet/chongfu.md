## [重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/description/)

> 给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

> ```
> 输入: s = "abab"
> 输出: true
> 解释: 可由子串 "ab" 重复两次构成。
> ```
>
> ```
> 输入: s = "aba"
> 输出: false
> ```
>
> ```
> 输入: s = "abcabcabcabc"
> 输出: true
> 解释: 可由子串 "abc" 重复四次构成。 (或子串 "abcabc" 重复两次构成。)
> ```



#### 自己想法



```c++
class Solution {
public:
    void getnext(int *next, string s){
        int j = 0;  //主串后缀，也是相等缀的长度
        next[0] = 0;
        for(int i=1; i<s.size(); i++){
            while(j>0 && s[j] != s[i])     j = next[j-1];
            if(s[j] == s[i])
                j++;
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern(string s) {
        //暴力解法，两层for循环，一个不断加入字母变成新子串
        //另一个复制子串，判断是否相等

        //还有一个利用库函数的解法
        //复制一份字符串，如果他是重复的，那么在去掉首尾字符串之后
        //他一定可以在中间再次找到这个原本的字符串

        if(s.size() == 0)
            return false;
        
        int next[s.size()];
        getnext(next, s);
        if(next[s.size()-1]!=0 && s.size() % (s.size() - next[s.size()-1]) == 0)
            return true;
        //这里只需要看最后一位的next数组就可以，只要存在，那么一定是最长的
        //
        return false;
    }
};
```

