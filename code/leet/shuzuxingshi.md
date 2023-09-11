#### [数组形式的整数加法](https://leetcode.cn/problems/add-to-array-form-of-integer/description/)

> 整数的 **数组形式**  `num` 是按照从左到右的顺序表示其数字的数组。
>
> - 例如，对于 `num = 1321` ，数组形式是 `[1,3,2,1]` 。
>
> 给定 `num` ，整数的 **数组形式** ，和整数 `k` ，返回 *整数 `num + k` 的 **数组形式*** 。

> ```
> 输入：num = [1,2,0,0], k = 34
> 输出：[1,2,3,4]
> 解释：1200 + 34 = 1234
> ```
>
> ```
> 输入：num = [2,7,4], k = 181
> 输出：[4,5,5]
> 解释：274 + 181 = 455
> ```
>
> ```
> 输入：num = [2,1,5], k = 806
> 输出：[1,0,2,1]
> 解释：215 + 806 = 1021
> ```



#### 自己想法

1. 一开始想把num全部转换成数字，但是位数太大了
2. 后来就想到把k转为vector，然后进行十进制加减
3. 通过讨论num和k长度的问题，选好基准，进行十进制
4. 但是要注意一点，1+999，这种，进行加减操作之后，还需要再遍历一遍，才能保证完全的正确

> 时间20ms，击败89%
>
> 内存26MB，击败87%

```c++
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& num, int k) {
        vector<int> k1;
        while(k!=0){
            int n=k%10;
            k1.insert(k1.begin(),n);        //从头插入
            k=k/10;
        }
        if(num.size()<=k1.size()){       //都在五位及以下
            for(int i=k1.size()-1,j=num.size()-1;j>=0;i--,j--){
                k1[i]=k1[i]+num[j];
                if(k1[i]>=10&&i-1>=0){
                    k1[i]=k1[i]-10;
                    k1[i-1]++;
                }
                else if(k1[i]>=10&&i-1<0){
                    k1[i]=k1[i]-10;
                    k1.insert(k1.begin(),1);
                    
                }
            }
            for(int q=k1.size()-1;q>=0;q--){        //再次检查一遍，防止1+999
                if(k1[q]>=10&&q-1>=0){  
                    k1[q]=k1[q]-10;
                    k1[q-1]++;

                }
                else if(k1[q]>=10&&q-1<0){
                    k1[q]=k1[q]-10;
                    k1.insert(k1.begin(),1);
                }
            }
            return k1;
        }

        else {      //num的位数大
            for(int i=k1.size()-1,j=num.size()-1;i>=0;i--,j--){
                num[j]=num[j]+k1[i];
                if(num[j]>=10){
                    num[j]=num[j]-10;
                    num[j-1]++;
                }
            }
            for(int q=num.size()-1;q>=0;q--){       //再次检查一遍，防止999+1
                if(num[q]>=10&&q-1>=0){
                    num[q]=num[q]-10;
                    num[q-1]++;
                }
                else if(num[q]>=10&&q-1<0){
                    num[q]=num[q]-10;
                    num.insert(num.begin(),1);
                }
            }
            return num;
        }
        return {0,0};
    }
};
```



#### 查看题解

1. 数字相加的类型总结

```c++
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& num, int k) {
        vector<int> cnt;

        int n1=num.size()-1;
        int carry=0;					//进位标志
        while(n1>=0||k!=0){				//判断结束条件，如果是两个vector就是都>=0
            int x= n1>=0 ? num[n1]:0;	//取出数字还是填补0
            int y= k!=0 ? k%10:0;

            int sum=x+y+carry;
            cnt.push_back(sum%10);		//取余数，保存进位
            carry=sum/10;

            n1--;
            k=k/10;
        }
        if(carry)
            cnt.push_back(1);				//最高位
        reverse(cnt.begin(),cnt.end());		//颠倒顺序
        return cnt;
    }
};
```



#### 总结

1. vector的位置插入
2. **数字相加的类型总结**
3. vector颠倒顺序