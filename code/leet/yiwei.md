## [有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

> 给定两个字符串 `*s*` 和 `*t*` ，编写一个函数来判断 `*t*` 是否是 `*s*` 的字母异位词。
>
> **注意：**若 `*s*` 和 `*t*` 中每个字符出现的次数都相同，则称 `*s*` 和 `*t*` 互为字母异位词。

> ```
> 输入: s = "anagram", t = "nagaram"
> 输出: true
> ```
>
> ```
> 输入: s = "rat", t = "car"
> 输出: false
> ```



#### 自己想法

1. map的添加元素方法，下标和insert
2. auto的map要用->first来指代

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        //auto it = map的，迭代器要用->first
        
        map<char, int> cnt1;
        map<char, int> cnt2;
        int length1 = 0, length2 = 0;       //长度，长度不同直接false
        for(int i=0; s[i]!=NULL; i++){      //map统计次数
            cnt1[s[i]]++;
            length1++;
        }
        for(int i=0; t[i]!=NULL; i++){
            cnt2[t[i]]++;
            length2++;
        }
        if(length1!=length2)
            return false;
        
        auto it1 = cnt1.begin(), it2 = cnt2.begin();    
        while(it1!=cnt1.end()){
            if(it1->first != it2->first || it1->second != it2->second)
                return false;
            it1++, it2++;
        }
        return true;
    }
};
```

- 哈希函数做法

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        //自己定义了哈希函数，将其映射到某个存储位置进行操作
        int cnt[26] = {0};
        for(int i=0; s[i]!=NULL; i++)
            cnt[s[i]-'a']++;
        for(int i=0; t[i]!=NULL; i++)
            cnt[t[i]-'a']--;

        for(int i=0; i<26; i++)
            if(cnt[i]!=0)
                return false;
        return true;
    }
};
```
