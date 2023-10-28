## [去除重复字母](https://leetcode.cn/problems/remove-duplicate-letters/description/)

> 给你一个字符串 `s` ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 **返回结果的字典序最小**（要求不能打乱其他字符的相对位置）。

> ```
> 输入：s = "bcabc"
> 输出："abc"
> ```
>
> ```
> 输入：s = "cbacdcbc"
> 输出："acdb"
> ```

#### 题解

```c++
class Solution {
public:
    int stack[10001];   //模拟栈
    int cur = 0; 
    int count[26] = {0};      //计数器
    bool flag[26] = {false};   //对应字符是否在栈里 
    //栈中保存结果，所以一个字母不能入栈两次
    
    void compute(string &s){
        //遍历阶段
        for(int i=0; i<s.size(); i++){
            int num = s[i]-'a';   //对应的字母表下标
            if(flag[num] == false){  //入栈的门槛，栈里没有它的元素
                while(cur > 0 && num <= stack[cur-1] && count[stack[cur-1]] > 0){
                    flag[stack[cur-1]] = false;
                    cur--;
                }
                stack[cur++] = num;
                flag[num] = true;
                count[num]--;       //入栈修改三件套
            }
            //当这个元素不进栈的时候，也要把count--
            else
                count[num]--;
        }
    }
    string removeDuplicateLetters(string s) {
        //先收集每个字符的出现次数
        //单调栈，弹出元素的时候，去判断后面还有没有它出现
        //没有的话，那就不能弹出了，必须在里面
        //一次来保证字典序最小，就是序列最晚

        //栈里保存的是最后的结果
        for(int i=0; i<s.size(); i++){
            count[s[i]-'a']++;
        }

        compute(s);
        //栈中cur指示的就是最后保存的元素个数
        //这里都是最后的答案，直接进行复制返回就可以
        string result;
        for(int i=0; i<cur; i++){
            result += (stack[i]+'a');
        }
        return result;
    }
};
```

