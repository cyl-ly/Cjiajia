## [有效的括号](https://leetcode.cn/problems/valid-parentheses/)

> 给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。
>
> 有效字符串需满足：
>
> 1. 左括号必须用相同类型的右括号闭合。
> 2. 左括号必须以正确的顺序闭合。
> 3. 每个右括号都有一个对应的相同类型的左括号。

> ```
> 输入：s = "()"
> 输出：true
> ```
>
> ```
> 输入：s = "()[]{}"
> 输出：true
> ```
>
> ```
> 输入：s = "(]"
> 输出：false
> ```



#### 自己想法



```c++
class Solution {
public:
    bool isValid(string s) {
        if(s.size() == 1)
            return false;

        stack<char> st;     //栈
        for(int i=0; i<s.size(); i++){
            if(s[i]=='[' || s[i]=='{' || s[i]=='(')
                st.push(s[i]);
            else {
                if(s[i]==')' &&!st.empty()&& st.top()=='(')     //要判断非空，不然上来就是右括号直接false
                    st.pop();
                else if(s[i]=='}' &&!st.empty()&& st.top()=='{')
                    st.pop();
                else if(s[i]==']' &&!st.empty()&& st.top()=='[')
                    st.pop();
                else 
                    return false;
            }
        }
        if(st.empty())
            return true;
        return false;
    }
};
```

