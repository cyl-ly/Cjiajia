## [反转字符串](https://leetcode.cn/problems/reverse-string/)

> 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。
>
> 不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

> ```
> 输入：s = ["h","e","l","l","o"]
> 输出：["o","l","l","e","h"]
> ```



#### 自己想法

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = 0;
        int j = s.size()-1;
        while(i<=j){
            swap(s[i++], s[j--]);
        }
    }
};
```

