## [单词规律](https://leetcode.cn/problems/word-pattern/)

> 给定一种规律 `pattern` 和一个字符串 `s` ，判断 `s` 是否遵循相同的规律。
>
> 这里的 **遵循** 指完全匹配，例如， `pattern` 里的每个字母和字符串 `s` 中的每个非空单词之间存在着双向连接的对应规律。

> ```
> 输入: pattern = "abba", s = "dog cat cat dog"
> 输出: true
> ```
>
> ```
> 输入:pattern = "abba", s = "dog cat cat fish"
> 输出: false
> ```
>
> ```
> 输入: pattern = "aaaa", s = "dog cat cat dog"
> 输出: false
> ```

#### 题解

1. 要理解题意

```c++
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        //先把单词提取出来，要注意长度有可能不等
        //双向哈希表匹配
        vector<string> word;
        string res;
        for(auto i:s){
            if(i == ' '){
                word.push_back(res);
                res = "";
            }
            else
                res += i;
        }
        word.push_back(res);
        if(pattern.size() != word.size())
            return false;

        unordered_map<char,string> mp1;
        unordered_map<string,char> mp2;
        for(int i=0; i<pattern.size(); i++){
            if(mp1.find(pattern[i]) != mp1.end() && mp1[pattern[i]] != word[i])    //先用find确定存在，再用下标访问
                return false;
            mp1[pattern[i]] = word[i];
        }
        //abba--dog dog dog dog这种情况只能用反向存储来排除了
        for(int i=0; i<pattern.size(); i++){
            if(mp2.find(word[i]) != mp2.end() && mp2[word[i]] != pattern[i])    
                return false;
            mp2[word[i]] = pattern[i];
        }
        return true;
    }
};
```

## 