## [同构字符串](https://leetcode.cn/problems/isomorphic-strings/)

> 给定两个字符串 `s` 和 `t` ，判断它们是否是同构的。
>
> 如果 `s` 中的字符可以按某种映射关系替换得到 `t` ，那么这两个字符串是同构的。
>
> 每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

> ```
> 输入：s = "egg", t = "add"
> 输出：true
> ```
>
> ```
> 输入：s = "foo", t = "bar"
> 输出：false
> ```
>
> ```
> 输入：s = "paper", t = "title"
> 输出：true
> ```

#### 题解

1. 和上一个单词规律很像，都需要双向匹配

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        if(s.size() != t.size())
            return false;

        unordered_map<char,char> mp;
        unordered_map<char,char> mp2;
        for(int i=0; i<s.size(); i++){
            if(mp.find(s[i])!=mp.end() && mp[s[i]] != t[i])
                return false;   //先存在再下标访问
            mp[s[i]] = t[i];
        }
        for(int i=0; i<s.size(); i++){
            if(mp2.find(t[i])!=mp2.end() && mp2[t[i]] != s[i])
                return false;   //调换位置遍历检查一下
            mp2[t[i]] = s[i];
        }
        return true;
    }
};
```

