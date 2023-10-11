## [反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/description/)

> 给你一个字符串 `s` ，请你反转字符串中 **单词** 的顺序。
>
> **单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。
>
> 返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。
>
> **注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

> ```
> 输入：s = "the sky is blue"
> 输出："blue is sky the"
> ```
>
> ```
> 输入：s = "  hello world  "
> 输出："world hello"
> 解释：反转后的字符串中不能存在前导空格和尾随空格。
> ```
>
> ```
> 输入：s = "a good   example"
> 输出："example good a"
> 解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
> ```



#### 自己想法

1. 利用了辅助空间On

```c++
class Solution {
public:
    void reverseString(string& s) {   //都是下标
        int i = 0;
        int j = s.size()-1; 
        while(i<=j){
            swap(s[i++], s[j--]);
        }
    }
    string reverseWords(string s) {
        int n = s.size();
        reverseString(s);       //整体进行翻转，返回再去翻转每一个单词
        cout<<s<<endl;
        string result;
        string c;       //临时存储
        for(int i=0; i<n; i++){
            if(s[i]!=' '){          //收集单词
                c+=s[i];
            }
            if((i>0 && s[i]==' ' && s[i-1]!=' ') || (i==n-1 && s[i]!=' ')){   
                //单词之间的空格就进行转换插入，或者是最后一个非空格进行插入
                reverseString(c);
                result += c+' ';
                c={};
            }
        }
        result.erase(result.end() - 1);     //删除最后一个空格
        return result;
    }
};
```

2. 空间复杂度O1，利用快慢指针直接原地移除空格

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
    string reverseWords(string s) {
        int n = s.size();
        reverseString(s, 0, n-1);       //整体进行翻转，返回再去翻转每一个单词
        //，利用快慢指针先删掉多余的空格
        int slow = 0;
        for(int i=0; i<n; i++){
            if(s[i]!=' '){      //这里是新的单词，因为每个单词挨着的位置下面会解决
                if(slow!=0)     s[slow++] = ' ';
                int begin = i;  //每个单词开头
                int end = i;	//去找到每个单词的结尾
                while(end<n && s[end]!=' ')     s[slow++] = s[end++];
                reverseString(s, begin, end-1);
                i = end;
            }
        }
        s.erase(s.begin()+slow, s.end());	//删掉多余的字符
        return s;
    }
};
```

