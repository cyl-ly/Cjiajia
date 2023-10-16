## [删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)

> 给出由小写字母组成的字符串 `S`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。
>
> 在 S 上反复执行重复项删除操作，直到无法继续删除。
>
> 在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

> ```
> 输入："abbaca"
> 输出："ca"
> 解释：
> 例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
> ```



#### 自己想法



```c++
class Solution {
public:
    string removeDuplicates(string s) {
        if(s.size() == 1)
            return s;
        
        stack<char> st;
        st.push(s[0]);
        for(int i=1; i<s.size(); i++){		//这里也可以用auto，但是要注意，他是从头开始遍历，就要改变判断条件了，前面也不能初始就进行入栈了
            if(!st.empty() && s[i] == st.top())
                st.pop();       //和栈顶字母相同就出栈
            else
                st.push(s[i]);
        }
        string cnt;
        while(!st.empty()){
            cnt = st.top()+cnt;
            st.pop();
        }
        return cnt;
    }
};
```

