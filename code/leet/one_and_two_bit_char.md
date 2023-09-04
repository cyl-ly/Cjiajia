### 1比特与2比特字符

> 有两种特殊字符：
>
> 第一种字符可以用一比特 0 表示
> 第二种字符可以用两比特（10 或 11）表示
>
> 给你一个以 0 结尾的二进制数组 bits ，如果最后一个字符必须是一个一比特字符，则返回 true
>
> ```
> 输入: bits = [1, 0, 0]
> 输出: true
> 解释: 唯一的解码方式是将其解析为一个两比特字符和一个一比特字符。
> 所以最后一个字符是一比特字符
> ```
>
> ```
> 输入：bits = [1,1,1,0]
> 输出：false
> 解释：唯一的解码方式是将其解析为两比特字符和两比特字符。
> 所以最后一个字符不是一比特字符
> ```


## 自己想法

> 执行用时：0 ms, 在所有 C++ 提交中击败了100.00% 的用户
>
> 内存消耗：9.4 MB, 在所有 C++ 提交中击败了66.34% 的用户

```c++
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {  
        int num=0;      //记录删掉的是几比特
        if(bits.size()==1&&bits[0]==0)
            return true;  
        
        int i=0;
        for(i=0;i<bits.size();){
            if(bits[i]==0){   //第一位是0，只能是一比特
                num=1;
                i++;		  //走一步
            }
            else if(bits[i]==1){    //一定是二比特
                    num=2;
                    i+=2;		//走两步
            }
        }
        if(i>=bits.size()&&num==1)
            return true;
        return false;
    }
};
```

1. 遇到0的话，一定是一比特，向下走一步
2. 遇到1的话，就一定是二比特，不用思考下一位元素，走两步
3. 用num记录走过最后一个是什么比特，这个题感觉很简单，没想到时间100%

## 总结

1. `bits.erase(bits.begin());`删除元素