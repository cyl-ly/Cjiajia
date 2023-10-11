## [找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

> 给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

> ```
> 输入：haystack = "sadbutsad", needle = "sad"
> 输出：0
> 解释："sad" 在下标 0 和 6 处匹配。
> 第一个匹配项的下标是 0 ，所以返回 0 。
> ```
>
> ```
> 输入：haystack = "leetcode", needle = "leeto"
> 输出：-1
> 解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
> ```



#### 自己想法

1. KMP算法

```c++
class Solution {
public:
    void getnext(int next[], string &s){
        //把i是后缀的结尾
        //j是前缀的结尾，同时也代表了最长相等前缀的长度
        int j = 0;
        next[j] = 0;        //next数组初始化
        for(int i=1; i<s.size(); i++){
            //不相等的话，就去递归的寻找上一位的数值
            while(j>0 && s[i] != s[j])  j = next[j-1];
            if(s[i] == s[j])    //相等的话，就是相等前缀长度+1
                j++;
            next[i] = j;        //并且存入到next数组
        }
    }
    int strStr(string haystack, string needle) {
        if(needle.size() == 0)
            return 0;
        
        int next[needle.size()];
        getnext(next, needle);      //得到next数组
        int i = 0;      //i是子串下标
        for(int j=0; j<haystack.size(); j++){   //j是主串，永不后退
            while(i>0 && haystack[j] != needle[i])
                i = next[i-1];      //这里有i-1了，所以条件i>0
            //不相等就让子串回退到next[i]
            
            if(haystack[j] == needle[i])
                i++;
            
            if(i == needle.size())
                return (j-needle.size()+1);
            //返回到主串的匹配下标
        }
        return -1;

    }
};
```

