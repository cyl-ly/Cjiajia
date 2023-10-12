## [目录]

### [28.KMP](找出字符串中第一个匹配项的下标)

---

### [找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

```c++
class Solution {
public:
    void getnext(int *next, string s){  //子串才需要next数组
    //二刷
        next[0] = 0;
        int j = 0;  //主串后缀，也是公共前缀的长度
        for(int i=1; i<s.size(); i++){
            while(j>0 && s[i] != s[j]){     //不断递归去寻找
                j = next[j-1];
            }
            if(s[i] == s[j])
                j++;    //主串向后移动一位，也是长度+1
            next[i] = j;    //更新next数组
        }
    }
    int strStr(string haystack, string needle) {
        if(needle.size() == 0)
            return -1;
        int next[needle.size()];    //子串才需要next数组
        getnext(next, needle);

        int i = 0;  //主串永不后退，所以在for循环里面的是主串
        for(int j=0; j<haystack.size(); ){
            while(i>0 && haystack[j] != needle[i])
                i = next[i-1];      
            //这里的while必须优先执行————二刷时候的错误
            //因为如果有不配的位置的话，会根据next数组找到下一会和匹配的位置，
            //这样下面的if条件可以直接进行匹配，而不需要再次进行循环
            
            if(haystack[j] == needle[i])    //相等，子串向后移动一位
                i++;
            
            if(i == needle.size())          //子串到达末尾
                return j-needle.size()+1;   //返回主串位置
        }
        return -1;
    }
};
```

